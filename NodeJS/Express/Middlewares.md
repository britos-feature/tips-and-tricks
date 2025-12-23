No Express, **middlewares** são funções intermediárias que têm acesso ao objeto de requisição (**`req`**), ao objeto de resposta (**`res`**) e ao próximo middleware da fila (**`next`**).

**Middlewares** servem para interceptar, processar ou modificar **requisições** antes que elas cheguem ao manipulador final da rota.

**Em termos técnicos:**

> Middleware é uma função que processa a requisição (`req`), a resposta (`res`) e decide se o fluxo continua (`next()`) ou termina (`res.send()`).

---

**Conceitos básicos:**
Um **middleware** é basicamente uma função com esta assinatura:

```js 

function (req, res, next) {
  // faz algo com a requisição ou resposta
  next(); // chama o próximo middleware
}

```

> **`next()`**
> 	- **É obrigatório** chamar **`next()`** quando você quer continuar o fluxo.
> 	- Se **não** chamar, a requisição **fica pendurada** (sem resposta).
> 	- Você pode **interromper o fluxo** se enviar uma resposta (**`res.send()`**, **`res.end()`**, **`res.json()`**).
<br>
### Fluxo de funcionamento

Quando o Express recebe uma requisição:

1. Ele passa essa requisição por uma **cadeia de middlewares**, na ordem em que foram definidos.
2. Cada middleware pode:
    - Modificar **`req`** ou **`res`**.
    - Encerrar a resposta (**`res.send()`**, **`res.json()`**, etc).
    - Ou chamar **`next()`** para passar o controle ao **próximo middleware**.
<br>
### Ordem de execução
A ordem de definição **importa muito!**

```js

app.use(middleware1);
app.use(middleware2);
app.get('/', (req, res) => res.send('Rota final'));

```
<br>
**Sequência de execução**

```sh

middleware1 → middleware2 → rota

```

> **Obs:.**
> 	Se `middleware1` **não chamar `next()`**, o fluxo **nunca chegará na rota**.
<br>


---

### Tipos de middlewares

1. **Middlewares de aplicação**  
    Aplicam-se globalmente a todas as rotas.  **`app.use()`**

```js

const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log(`Método: ${req.method}, URL: ${req.url}`);
  next();
});

```
<br>
2. **Middlewares específicos de rotas**

```js

app.get('/user/:id', (req, res, next) => {
  console.log('Middleware da rota /user');
  next();
}, (req, res) => {
  res.send('Usuário acessado!');
});

```
<br>
3. **Middlewares embutidos do Express**
	- `express.json()` → interpreta o corpo JSON.
	- `express.urlencoded()` → interpreta dados de formulários HTML.
	- `express.static()` → serve arquivos estáticos.

```js

// exemplos:

app.use(express.json());
app.use(express.static('public'));

```
<br>
4. **Middlewares de tratamento de erros**
	Esses têm **4 parâmetros**: `(err, req, res, next)`

```js

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Algo deu errado!');
});

```
<br>
### Resumo rápido

| Tipo       | Onde age                   | Exemplo                        |
| ---------- | -------------------------- | ------------------------------ |
| Aplicação  | Em todas as rotas          | `app.use(middleware)`          |
| Específico | Em uma rota                | `app.get('/rota', middleware)` |
| Embutido   | Funções nativas do Express | `express.json()`               |
| Terceiros  | Pacotes externos           | `cors()`, `morgan()`           |
| Erros      | Captura falhas             | `(err, req, res, next)`        |
<br>

---
---

### Exemplo completo prático.

## Estrutura do exemplo

Teremos 5 camadas de middlewares:

1. 🌐 **Middleware global** (executa para todas as rotas)
2. 📅 **Middleware que adiciona informações ao `req`**
3. 🔒 **Middleware de autenticação** (simulado)
4. 🚀 **Rota final com resposta**
5. 💥 **Middleware de tratamento de erros**

```js

const express = require('express');
const app = express();

// 1️⃣ Middleware global (executa em todas as rotas)
app.use((req, res, next) => {
  console.log('🔹 Middleware Global executado');
  console.log(`Método: ${req.method} | URL: ${req.url}`);
  next(); // passa para o próximo middleware
});

// 2️⃣ Middleware que adiciona dados ao objeto req
app.use((req, res, next) => {
  req.requestTime = new Date().toISOString();
  console.log('⏰ Middleware adicionou o tempo da requisição');
  next();
});

// 3️⃣ Middleware de autenticação (simulação)
app.use((req, res, next) => {
  const autorizado = req.headers.authorization === '12345';
  if (!autorizado) {
    console.log('🚫 Acesso negado: token inválido');
    return res.status(401).json({ erro: 'Não autorizado' });
  }
  console.log('✅ Usuário autenticado');
  next();
});

// 4️⃣ Rota principal
app.get('/dados', (req, res) => {
  console.log('📦 Rota /dados executada');
  res.json({
    mensagem: 'Dados recebidos com sucesso!',
    horario: req.requestTime
  });
});

// 5️⃣ Middleware de tratamento de erro
app.use((err, req, res, next) => {
  console.error('💥 Erro capturado no middleware de erro:', err.message);
  res.status(500).json({ erro: 'Erro interno do servidor' });
});

// Servidor
app.listen(3000, () => {
  console.log('🚀 Servidor rodando na porta 3000');
});

```
<br>
### Entendimento!

 - **Request**

```sh

GET /dados

```
<br>

---
---

- **Cenário 1 (sem token)

```sh

curl http://localhost:3000/dados

```
<br>
- **Saída**

```sql

🔹 Middleware Global executado
⏰ Middleware adicionou o tempo da requisição
🚫 Acesso negado: token inválido

```

> O fluxo parou no middleware de autenticação — **não chegou na rota**.
<br>
- **Cenário 2 (com token)

```sh

curl -H "Authorization: 12345" http://localhost:3000/dados

```
<br>
- **Saída**

```sql

🔹 Middleware Global executado
⏰ Middleware adicionou o tempo da requisição
✅ Usuário autenticado
📦 Rota /dados executada

```

> Agora o fluxo passou **por todos os middlewares**, chegou na rota e enviou a resposta JSON.

<br>
### Simulando um erro

- **Se dentro de qualquer middleware ou rota você fizer**

```js

next(new Error('Falha simulada'));

```

> O Express **pulará direto para o middleware de erro** (`app.use((err, req, res, next) => { ... })`).
<br>

---
---

### Resumo visual do fluxo

```css

Requisição
   ↓
[Middleware Global]
   ↓
[Middleware adiciona req.requestTime]
   ↓
[Middleware Autenticação]
   ↓ (se autorizado)
[Rota /dados]
   ↓
Resposta JSON
   ↓
[Middleware de erro] (se ocorrer exceção)

```
<br>
### Dica avançada

Você também pode criar **grupos de middlewares específicos** para certas rotas usando o **Router** do Express:

```js

const router = express.Router();

router.use((req, res, next) => {
  console.log('🔸 Middleware do grupo /admin');
  next();
});

router.get('/painel', (req, res) => {
  res.send('Área administrativa');
});

app.use('/admin', router);

```
<br>
