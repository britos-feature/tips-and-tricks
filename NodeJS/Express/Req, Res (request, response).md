## **`req`** -> (request) – Objeto de requisição

<big><code><b>req</b></code></big>,  representa **tudo que o cliente (navegador ou outro serviço) envia para o servidor**.
Ele contém as propriedades que trazem informações sobre:

- **`req.app`** -> Referência à instância do Express. Útil em middlewares.
- **`req.baseUrl`** -> Caminho base da rota atual. Importante para **routers**.
- **`req.originalUrl`** -> URL completa original da requisição, antes de qualquer middleware modificar.
- **`req.path`** -> Caminho da requisição (sem query string). Ex: **`/user/123`.**
- **`req.hostname`** -> Nome do host da requisição.
- **`req.ip`** -> Endereço IP do cliente.
- **`req.ips`** -> Array de IP caso o app esteja atrás de proxies (com **`trust proxy`**).
- **`req.protocol`** -> Protocolo usado (`http` ou `https`).
- **`req.secure`** -> **`true`** se a requisição for HTTPS.
- **`req.cookies`** -> Cookies enviados pelo cliente (requer **`cookie-parser`**).
- **`req.signedCookies`** -> Cookies assinados (usando **`cookie-parser`**).
- **`req.get(headerName)`** -> Retorna o valor de um cabeçalho específico.
- **`req.accepts(types)`** -> Verifica se o cliente aceita certos **tipos MIME.**
- **`req.is(type)`** -> Verifica o tipo do conteúdo enviado (**Content-Type**).
<br>
**Métodos úteis**

- **`req.get('Content-Type')`** → Pega um header específico.  
- **`req.param(name)`** → Retorna um parâmetro (params, body ou query) — **deprecated**, mas às vezes útil.
- **`req.range(size)`** → Retorna intervalos de bytes de arquivos (útil para streaming ou downloads parciais).
<br>
---

**Exemplo das principais propriedades**

- **`req.params`**
	- Contém os parâmetros de rota (definidos com `:param`).

```js

app.get('/user/:id', (req, res) => {
  console.log(req.params.id); // id passado na URL
});

```
<br>
- ****`req.query`**  
	- Contém os parâmetros de query string (URL depois do `?`).

```js

app.get('/search', (req, res) => {
  console.log(req.query.term); // ?term=javascript
});

```
<br>
- ****`req.body`**  
	- Contém o corpo da requisição (geralmente usado em POST/PUT). Precisa do middleware **`express.json()`** ou **`express.urlencoded()`**.

```js

app.use(express.json());
app.post('/login', (req, res) => {
  console.log(req.body.username);
});

```
<br>
- **`req.headers`**
	- Objeto com os cabeçalhos da requisição.

```js

console.log(req.headers['content-type']);

```
<br>

----

- **`req.method`**
	- O **`req.method`** é uma propriedade do objeto `request` (**`req`**) no **Express**, pois ela indica **qual método HTTP** foi usado na requisição (`GET`, `POST`, `PUT`, `DELETE` ou `PATCH`). Isso permite que você **trate uma mesma rota de formas diferentes**, dependendo do método usado.

1. **Usando `req.method` em uma rota genérica**

```js

const express = require('express');
const app = express();

app.use(express.json());

app.all('/user', (req, res) => {
  if (req.method === 'GET') {
    res.send('Você fez uma requisição GET para /user');
  } else if (req.method === 'POST') {
    res.send('Você fez uma requisição POST para /user');
  } else if (req.method === 'PUT') {
    res.send('Você fez uma requisição PUT para /user');
  } else if (req.method === 'DELETE') {
    res.send('Você fez uma requisição DELETE para /user');
  } else {
    res.status(405).send(`Método ${req.method} não permitido.`);
  }
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

```

> - `GET /user` → retorna “Você fez uma requisição GET para /user”
> - `POST /user` → retorna “Você fez uma requisição POST para /user”
   etc.  O método **`req.method`** retorna **sempre uma string em maiúsculas** (`"GET"`, `"POST"`, etc.).
<br>
2. **Middleware que reage conforme o método `req.method`**

```js

app.use((req, res, next) => {
  console.log(`Método utilizado: ${req.method} - URL: ${req.url}`);
  next(); // continua o fluxo
});

app.get('/', (req, res) => res.send('Rota GET'));
app.post('/', (req, res) => res.send('Rota POST'));

```

> Ótimo para **logar ou auditar requisições** no servidor.
<br>
3. **Usando `req.method` com verificação dinâmica**

```js

app.all('/api/data', (req, res) => {
  switch (req.method) {
    case 'GET':
      res.json({ msg: 'Obtendo dados...' });
      break;
    case 'POST':
      res.json({ msg: 'Criando novo recurso...' });
      break;
    case 'PUT':
      res.json({ msg: 'Atualizando recurso...' });
      break;
    case 'DELETE':
      res.json({ msg: 'Removendo recurso...' });
      break;
    default:
      res.status(405).json({ error: 'Método não suportado.' });
  }
});

```
<br>
4. **Filtro de métodos não permitidos**

```js

app.all('/admin', (req, res) => {
  if (req.method !== 'POST') {
    return res.status(403).send(`Método ${req.method} não permitido nesta rota.`);
  }
  res.send('Acesso autorizado via POST.');
});

```

> Útil quando você quer **restringir** rotas a apenas um método (por exemplo, apenas `POST` para formulários).
<br>
---

- **`req.url`**
	- A propriedade **`req.url`** retorna **a URL completa da requisição**, incluindo:
		- o **caminho** da rota
		- e a **query string** (parâmetros após o `?`)

Útil para **logar requisições**, **criar middlewares genéricos**, ou **fazer verificações dinâmicas de rota**.

- **Usando `req.url` para registrar acessos**

```js

const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log(`${req.method} em ${req.url}`);
  next(); // passa para a próxima rota
});

app.get('/user', (req, res) => {
  res.send('Rota /user acessada!');
});

app.get('/search', (req, res) => {
  res.send('Rota /search acessada!');
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

```

> **`req.url`** mostra a rota e também a query string se houver (**`/search?term=node`**).
<br>
- **Redirecionando dinamicamente com base em `req.url`**

```js

app.use((req, res, next) => {
  if (req.url === '/old-page') {
    return res.redirect('/new-page');
  }
  next();
});

app.get('/new-page', (req, res) => res.send('Você foi redirecionado!'));

```
<br>

---

- **req.cookies**
	- O `req.cookies` é um **objeto contendo todos os cookies** enviados pelo cliente na requisição. Para usar essa propriedade, é **necessário instalar e ativar o middleware `cookie-parser`**.

```sh

npm install cookie-parser

```
<br>
 -  **Lendo cookies com `req.cookies`**

```js

const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

// habilita suporte a cookies
app.use(cookieParser());

// cria um cookie
app.get('/set-cookie', (req, res) => {
  res.cookie('user', 'Britos', { maxAge: 60000, httpOnly: true });
  res.send('Cookie criado!');
});

// lê cookies
app.get('/get-cookie', (req, res) => {
  console.log(req.cookies); // { user: 'Britos' }
  res.send(`Usuário salvo no cookie: ${req.cookies.user}`);
});

// remove cookie
app.get('/clear-cookie', (req, res) => {
  res.clearCookie('user');
  res.send('Cookie removido!');
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

```

> **Explicação***
> 	- **`req.cookies.user`** → lê o cookie chamado `"user"`
> 	-  **`res.cookie(name, value, options)`** → cria um cookie
> 	-  **`res.clearCookie(name)`** → apaga o cookie
<br>
- **Middleware que combina `req.url` + `req.cookies`**
	- Aqui veremos os dois juntos funcionando na prática

```js

app.use(cookieParser());

// Middleware para auditoria
app.use((req, res, next) => {
  const user = req.cookies.user || 'visitante';
  console.log(`[AUDITORIA] ${user} acessou ${req.url} via ${req.method}`);
  next();
});

// Rotas
app.get('/home', (req, res) => res.send('Página inicial'));
app.get('/profile', (req, res) => res.send('Página do perfil'));

```

```sh

# Saída no console.

[AUDITORIA] Britos acessou /profile via GET
[AUDITORIA] visitante acessou /home via GET

```
<br>
- **Cookies assinados (`req.signedCookies`)**
	- Você pode criar cookies **seguros e assinados** com uma chave secreta:

```js

const express = require('express');
const cookieParser = require('cookie-parser');
const app = express();

// inicializa cookie-parser com chave secreta
app.use(cookieParser('chave-super-secreta'));

// define cookie assinado
app.get('/set-signed', (req, res) => {
  res.cookie('sessionId', 'abc123', { signed: true });
  res.send('Cookie assinado criado!');
});

// lê cookie assinado
app.get('/check-signed', (req, res) => {
  res.json({
    normal: req.cookies,        // cookies comuns
    signed: req.signedCookies,  // cookies assinados
  });
});

```

> **Diferencial:**
> 	- Se alguém tentar alterar o valor do cookie manualmente, ele será **invalidado automaticamente**, protegendo a sessão.
<br>

---
---

## **`res`** -> (response) – Objeto de resposta

O **`res`** representa **o que o servidor envia de volta para o cliente**. Ele tem métodos para controlar o retorno da requisição. Ele é super poderoso e flexível.

- **`res.send(body)`** -> Envia uma resposta (texto, Buffer, JSON, etc.)
	- **ex:**  `js res.send('Olá mundo');`
- **`res.json(obj)`** -> Envia um objeto como JSON
	- **ex:**  `js res.json({ user: 'Britos' });`
- **`res.status(code)`** -> Define o status HTTP da resposta
	- **ex:** `js res.status(404).send('Não encontrado');`
- **`res.type(type)`** -> Define o tipo MIME da resposta
	- **ex:** `js res.type('json').send({ msg: 'ok' });`
- **`res.set(field, value)`** -> Define um cabeçalho HTTP
	- **ex:** `js res.set('Content-Type', 'text/html');`
- **`res.get(field)`** -> Retorna valor de um cabeçalho
	- **ex:** `js console.log(res.get('Content-Type'));`
- **`res.redirect([status], url)`** -> Redireciona o cliente
	- **ex:** `js res.redirect('/login');`
- **`res.cookie(name, value, [options])`** -> Define um cookie
	- **ex:** `js res.cookie('token', 'abc123', { httpOnly: true });`
- **`res.clearCookie(name)`** -> Remove um cookie
	- **ex:** `js res.clearCookie('token');`
- **`res.render(view, [locals])`** -> Renderiza uma view (EJS, Pug, etc.)
	- **ex:** `js res.render('index', { title: 'Home' });`
- **`res.sendFile(path, [options], [callback])`** -> Envia um arquivo
	- **ex:** `js res.sendFile(__dirname + '/index.html');`
- **`res.download(path, [filename], [callback])`** -> Força o download de um arquivo
	- **ex:** `js res.download('./files/report.pdf');`
- **`res.links(links)`** -> Define cabeçalhos `Link` (para API HATEOAS)
	- **ex:** `js res.links({ next: '/page/2', last: '/page/10' });`
- **`res.location(url)`** -> Define o cabeçalho `Location`
	- **ex:** `js res.location('/new-resource');`
- **`res.end([data])`** -> Encerra a resposta (opcionalmente com dados)
- **ex:** `js res.end('Fim da resposta');`
<br>
**Métodos úteis**

- **`res.locals`** -> Objeto para armazenar dados compartilhados com views ou middlewares
	- **ex:** `js res.locals.user = 'Britos';`
- **`res.headersSent`** -> **`true`** se os cabeçalhos já foram enviados
	- **ex:** `js if(res.headersSent) console.log('Enviado');`
- **`res.app`** -> Instância do aplicativo Express
	- **ex:** `js console.log(res.app.get('env'));`


