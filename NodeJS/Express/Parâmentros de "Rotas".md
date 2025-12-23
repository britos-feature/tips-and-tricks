Os **parâmetros de rota** (ou **route parameters**) são **valores dinâmicos** inseridos diretamente **na URL** que podem ser acessados e utilizados dentro do código da rota.

Os **parâmetros de rota** permitem ao servidor identificar **recursos específicos** sem precisar criar uma rota estática para cada um.

### [[#^89db56|req.params]], [[#^73704f|req.query]], [[#^451b86|req.body]]
<br>
**💬 Pense assim:**

> Em vez de criar uma rota pra cada usuário:
> 	- `/usuarios/1`
> 	- `/usuarios/2`
> 	- `/usuarios/3`
<br>

**Criamos assim:**

```js

app.get("/user/id:", ...)

// o "id:" funciona como uma variável de rota. 
```
<br>

---

## `REQ.PARAMS`

^89db56

**Interpretação dos parâmetros de "rotas"**

Quando o Express lê uma rota com **`:param`**, ele:

1. Mapeia o padrão da URL.
2. Extrai o valor que vem na posição do parâmetro.
3. Armazena todos os parâmetros no objeto **`req.params`** (um **objeto JavaScript** simples).
<br>
**Exemplo**

- **Único parâmetro**
	- Definição de um **único parâmetro** na rota.

```sh

# URL
http://localhost:3000/product/42

```
<br>
```js

app.get('/product/:id', (req, res) => {
  console.log(req.params);
  res.send(req.params);
});

```
<br>
```js

// return Object "req.params"
{ id: "42" }

```
<br>

---

- **Múltiplos parâmetros**
	- Você pode definir **quantos parâmetros quiser** na rota.

```sh

# URL
http://localhost:3000/lojas/sp/sao-paulo/15

```
<br>
```js

app.get('/lojas/:state/:city/:id', (req, res) => {
  console.log(req.params);
  res.send(req.params;
});

```
<br>
```js

// return Object "req.params"
{ state: "sp", city: "sao-paulo", id: "15" }

```
<br>

----

-  <big><b>`?`</b></big> **Parâmetro opcional**
	- O ponto de exclamação " **`?`** ", torna o parâmetro **opcional**, ou seja, a rota funciona **com ou sem o parâmetro inserido.

```js

app.get('/clientes/:nome?', (req, res) => {
  const nome = req.params.nome || 'Visitante';
  res.send(`Olá, ${nome}!`);
});

```

> **Funcionará com:**
> 	- **`/clientes`** -> Olá Visitante!
> 	- **`/clientes/Jonh`** -> Olá Jonh!
<br>
---


- **Parâmetros com REGEX** (Regular expression)
	- Você pode restringir **o formato do parâmetro** com **expressões regulares** diretamente na rota.

```js

// Regex aceitar apenas números no parâmetro "id"
app.get('/pedido/:id(\\d+)', (req, res) => {
  res.send(`Pedido número ${req.params.id}`);
});

```

> 	- **`/pedido/123`** -> URL: nesse formato **OK** (number)
> 	- **`/pedido/abc`** -> URL: nesse formato **Not Accepted** (caracteres)
<br>
---

- **Parâmetros utilizando curinga <big><b>" * "</b></big> - (catch all)**
	- Captura **tudo que vier depois da rota**

```js

app.get('/files/*', (req, res) => {
  res.send(`Caminho solicitado: ${req.params[0]}`);
});

```

> **URL:**  ... /files/docs/man.pdf -> caminho solicitado atendido
<br>
---

- **Parâmetros com middleware `app.param()`**
	- Permite executar lógica **antes da rota**

```js

app.param('id', (req, res, next, id) => {
  console.log(`ID recebido: ${id}`);
  next();
});

app.get('/usuarios/:id', (req, res) => {
  res.send(`Usuário: ${req.params.id}`);
});

```

> Útil para validação, conversão de tipos, carregamento de recursos do banco.
<br>
---

### Resumo das operações de parâmetros

| Tipo       | Exemplo                        | Obrigatório | Observações                              |
| ---------- | ------------------------------ | ----------- | ---------------------------------------- |
| Básico     | `/:id`                         | Sim         | Valor único, obrigatório                 |
| Opcional   | `/:nome?`                      | Não         | Pode ou não existir                      |
| Múltiplos  | `/:categoria/:id`              | Sim         | Vários valores obrigatórios              |
| Regex      | `/:id(\\d+)`                   | Sim         | Restringe formato (números, letras etc.) |
| Curinga    | `/*`                           | Sim         | Captura todo o restante da URL           |
| Combinado  | `/:estado/:cidade?/:id(\\d+)?` | Parcial     | Combina tipos diferentes                 |
| Middleware | `app.param('id', ...)`         | N/A         | Executa lógica antes da rota             |
<br>

---
---

## `REQ.QUERY`

^73704f

O objeto **`req.query`** contém **todos os parâmetros passados na URL após o sinal de interrogação (`?`)**, também conhecidos como **query strings**.

Esses parâmetros são enviados como **pares chave=valor**, e servem para **filtrar, ordenar ou controlar** a resposta da rota — **sem alterar o caminho da rota em si**.

**Funcionamento:**
	O Express extrai automaticamente **os parâmetros da query string** da URL — isto é, tudo o que vem **após o “?”** — e transforma em um **objeto JavaScript simples**.

**Exemplo**

- Estrutura da **URL**

```sh

.../rota?chave1=valor1&chave2=valor2`

```
<br>
- Interpretação

```js

req.query = {
  chave1: 'valor1',
  chave2: 'valor2'
}

```
<br>
### **Quando e porque utilizar `req.query`

- Para utilização de **Filtros** -> `/produtos?categoria=livros`
	- **return:** `categoria = “livros”`
	
- Para **Paginação** -> `/usuarios?page=2&limit=10`
	- **return:** `page = 2, limit = 10`
	
- Para **Ordenação** -> `/filmes?sort=ano&ordem=desc`
	- **result:** `sort = “ano”, ordem = “desc”`
	
- Para **Busca** -> `/posts?q=nodejs`
	- **result:** `q = “nodejs”`
	
- Para **Controle de exibição** -> `/relatorios?modo=compacto`
	- **result:** `modo = “compacto”`

--
> Use `req.query` quando a informação **não identifica um recurso específico**, mas sim **modifica o resultado**.
<br>
---

### **Exemplo pratico com vários parâmetros**

```js

app.get("/produtos", (req, res) => {
	const {
		categoria,
		precoMin,
		precoMax,
		page = 1,
		limit = 10,
		sort = "nome",
	} = req.query;
	
	res.json({
		categoria,
		precoMin,
		precoMax,
		page: Number(page),
		limit: Number(limit),
		sort,
	});
});

```
<br>
- **URL**

```sh

http://localhost:3000/produtos?categoria=eletronicos&precoMin=1000&precoMax=3000&page=2&sort=preco

```
<br>
- **Resposta**

```js

{
  "categoria": "eletronicos",
  "precoMin": "1000",
  "precoMax": "3000",
  "page": 2,
  "limit": 10,
  "sort": "preco"
}

```

> **Detalhes importantes:**
> 	-  Você pode definir **valores padrão** direto no destructuring (`page = 1`).
> 	-  O Express converte tudo em **string**, então valores numéricos precisam ser convertidos (`Number(page)`).
<br>

### **Tipos de valores suportados na `URL`**

**Valores:**                                         **URL**                                           **`req.query`**

**String simples**                            `?nome=Ana`                                 `{ nome: 'Ana' }`
**Número (texto)**                           `?idade=30`                                 `{ idade: '30' }`
**Booleano (texto)**                        `?ativo=true`                              `{ ativo: 'true' }`
**Múltiplos valores**                       `?tag=node&tag=express`       `{ tag: ['node', 'express'] }`
**Objeto (forma JSON)**                `?filtro[preco]=1000`            `{ filtro: { preco: '1000' } }`
<br>
### Configurando o parser de query (avançado)

Por padrão, o Express usa o módulo nativo **`querystring`**, que **não interpreta objetos complexos**.  
Para habilitar suporte a queries como `?filtro[categoria]=eletronicos`, você pode mudar o parser para `qs`:

```js

const express = require('express');
const qs = require('qs');

const app = express();

// Configurar parser customizado
app.set('query parser', str => qs.parse(str));

app.get('/produtos', (req, res) => {
  res.json(req.query);
});

```
<br>
- **URL**

```sh

/produtos?filtro[categoria]=livros&filtro[estoque]=true

```
<br>
- **Resposta**

```json

{
  "filtro": {
    "categoria": "livros",
    "estoque": "true"
  }
}

```
<br>

---

### Combinando `req.query` com `req.params` e `req.body`

|Origem|Onde aparece|Exemplo|Acesso|
|---|---|---|---|
|**Rota (req.params)**|Caminho da URL|`/usuarios/:id` → `/usuarios/10`|`req.params.id`|
|**Query (req.query)**|Após “?” na URL|`/usuarios?ativo=true`|`req.query.ativo`|
|**Body (req.body)**|Corpo da requisição|JSON ou formulário|`req.body.nome`|




```js

app.put('/usuarios/:id', (req, res) => {
  const { id } = req.params;
  const { ativo } = req.query;
  const { nome, email } = req.body;

  res.json({ id, ativo, nome, email });
});

```
<br>
- **Request**

```sh

PUT /usuarios/25?ativo=true
Body: { "nome": "Lucas", "email": "lucas@gmail.com" }

```
<br>
- **Resposta**

```json

{
  "id": "25",
  "ativo": "true",
  "nome": "Lucas",
  "email": "lucas@gmail.com"
}

```
<br>

----
----

## `REQ.BODY`

^451b86

O objeto **`req.body`** contém **os dados enviados no corpo (body) da requisição HTTP** — geralmente em **métodos como POST, PUT, PATCH ou DELETE**.

Esses dados são enviados **pelo cliente (front-end, API, formulário, etc.)** e representam **informações que você quer gravar, atualizar ou processar** no servidor.

**`req.body`**, nada mais é que **envio de dados** em requisições HTTP. Diferente de **`req.params`** (dados na rota) e **`req.query`** (dados na URL).
<br>
**Exemplos de onde ele é usado:**

- Enviar um formulário HTML.
- Enviar um JSON via API.
- Fazer upload de um arquivo.
- Atualizar dados em um PUT/PATCH.

### Request `HTTP`

- Exemplo: Requisição **POST** com corpo JSON

```sh

POST /usuarios HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Content-Length: 34

{
  "nome": "João",
  "idade": 25
}

```

> A parte depois do cabeçalho (`Content-Length`) é o **corpo**.
> O Express **não entende isso automaticamente** — ele precisa de um _parser_.
<br>
### Exemplo básico

- **Criando uma rota que recebe dados via POST**:

```js

const express = require('express');
const app = express();

// Middleware necessário para ler JSON do corpo da requisição
app.use(express.json());

app.post('/usuarios', (req, res) => {
  console.log(req.body);
  res.send(`Usuário ${req.body.nome} cadastrado com sucesso!`);
});

app.listen(3000, () => console.log('Servidor rodando em http://localhost:3000'));

```
<br>
- **Enviando request com o corpo**

```json

{
  "nome": "Ana",
  "idade": 28
}

```
<br>
- **Saída**

```txt

Usuário Ana cadastrado com sucesso!

```
<br>

- **`req.body`**, conterá:

```js

{ nome: 'Ana', idade: 28 }

```

---

### Entendimento Express `req.body`

Quando uma requisição chega, o **Express** precisa interpretar o conteúdo binário do corpo e convertê-lo para **JavaScript** _(JSON, objeto, string, etc.)_. Isso é feito por **middlewares** chamados **body parsers**. 

Sem os **middlewares**, o **`req.body`** será undefined.

#### Middlewares necessários

| Tipo de dado recebido         | Middleware necessário                    | Content-Type esperado               |
| ----------------------------- | ---------------------------------------- | ----------------------------------- |
| JSON                          | `express.json()`                         | `application/json`                  |
| Formulário simples (HTML)     | `express.urlencoded({ extended: true })` | `application/x-www-form-urlencoded` |
| Arquivos (imagens, PDFs etc.) | `multer`                                 | `multipart/form-data`               |

**Exemplo:**

```js

const express = require('express');
const app = express();

// Permite receber JSON e formulários
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

```

> Agora qualquer dado enviado será processado e estará disponível em **`req.body`**.
<br>
---

### Exemplo prático completo

- **Servidor**

```js

const express = require('express');
const app = express();

app.use(express.json()); // habilita JSON
app.use(express.urlencoded({ extended: true })); // habilita formulários

app.post('/cadastro', (req, res) => {
  const { nome, email, idade } = req.body;
  res.json({
    mensagem: 'Cadastro recebido!',
    dados: { nome, email, idade }
  });
});

app.listen(3000, () => console.log('Servidor rodando em http://localhost:3000'));

```
<br>
	- 1. **Enviando via JSON (Postman ou API)**
		- Método: `POST`
		- URL: `http://localhost:3000/cadastro`
		- Body (JSON):

```json

{
  "nome": "Maria",
  "email": "maria@gmail.com",
  "idade": 22
}

```
<br>
	- **Response**

```json

{
  "mensagem": "Cadastro recebido!",
  "dados": {
    "nome": "Maria",
    "email": "maria@gmail.com",
    "idade": 22
  }
}

```
<br>
	- 2. **Enviando via Formulário HTML**

```html

<form action="/cadastro" method="POST">
  <input name="nome" placeholder="Nome">
  <input name="email" placeholder="Email">
  <input name="idade" placeholder="Idade">
  <button type="submit">Enviar</button>
</form>

```
<br>
	- **Navegador envia os dados como:**

```ini

nome=Maria&email=maria@gmail.com&idade=22

```
<br>
	- **Express converte automaticamente para:**

```js

req.body = { nome: 'Maria', email: 'maria@gmail.com', idade: '22' }

```
<br>
	- 3. **Enviando via curl (linha de comando)*

```sh

curl -X POST http://localhost:3000/cadastro \
  -H "Content-Type: application/json" \
  -d '{"nome":"Maria","email":"email@email.com", "idade":23}'

```
<br>

----

### Formatos aceitos `req.body`

1. JSON
	- Padrão moderno.
	- Ideal para APIs REST.

**Exemplo:**

```json

{ "produto": "Teclado", "preco": 150 }

```
<br>
2. **x-www-form-urlencoded**
	- Padrão de formulários HTML.

**Exemplo:**

```sh

produto=Teclado&preco=150

```
<br>
3. **multipart/form-data**
	-  Usado para upload de arquivos.
	-  Precisa do middleware **`multer`**.

**Exemplo:**

```sh

Content-Type: multipart/form-data

```
<br>

---

### Diferença entre req.body, req.params e req.query

| Tipo           | Onde fica              | Exemplo de rota               | Como acessar      | Uso comum                |
| -------------- | ---------------------- | ----------------------------- | ----------------- | ------------------------ |
| **req.params** | Na rota                | `/usuarios/:id`               | `req.params.id`   | Identificar um recurso   |
| **req.query**  | Após o “?” na URL      | `/usuarios?ativo=true&page=2` | `req.query.ativo` | Filtros, paginação       |
| **req.body**   | No corpo da requisição | POST `/usuarios`              | `req.body.nome`   | Criar ou atualizar dados |
<br>
### Erros comuns com `req.body`

| Erro                             | Causa                                             | Solução                                 |
| -------------------------------- | ------------------------------------------------- | --------------------------------------- |
| `req.body` é `undefined`         | Faltou `express.json()` ou `express.urlencoded()` | Adicione `app.use(express.json())`      |
| O corpo vem vazio                | O `Content-Type` não bate com o formato enviado   | Verifique o cabeçalho da requisição     |
| Recebe strings em vez de números | Tudo vem como texto                               | Converta com `Number()` ou `parseInt()` |
| Problemas com upload             | Usando JSON ao invés de `multipart/form-data`     | Use o middleware `multer`               |
<br>
# Validação de `req.body`

Antes de salvar dados, **valide o conteúdo**.  

**Exemplo simples:**

```js

app.post('/login', (req, res) => {
  const { usuario, senha } = req.body;

  if (!usuario || !senha) {
    return res.status(400).json({ erro: 'Usuário e senha são obrigatórios!' });
  }

  res.json({ mensagem: 'Login efetuado!' });
});

```

> **Resultado:**
> 	- Se faltar algum campo → erro `400`.
> 	- Se tudo estiver ok → login aceito.
<br>
### Exemplo real de API REST com `req.body`

```js

const express = require('express');
const app = express();
app.use(express.json());

let usuarios = [];

// Criar novo usuário
app.post('/usuarios', (req, res) => {
  const { nome, email } = req.body;

  if (!nome || !email) {
    return res.status(400).json({ erro: 'Nome e email são obrigatórios!' });
  }

  const novo = { id: usuarios.length + 1, nome, email };
  usuarios.push(novo);

  res.status(201).json({ mensagem: 'Usuário criado com sucesso!', usuario: novo });
});

// Listar usuários
app.get('/usuarios', (req, res) => {
  res.json(usuarios);
});

// Atualizar usuário
app.put('/usuarios/:id', (req, res) => {
  const { id } = req.params;
  const { nome, email } = req.body;

  const usuario = usuarios.find(u => u.id == id);
  if (!usuario) return res.status(404).json({ erro: 'Usuário não encontrado!' });

  usuario.nome = nome || usuario.nome;
  usuario.email = email || usuario.email;

  res.json({ mensagem: 'Usuário atualizado com sucesso!', usuario });
});

app.listen(3000, () => console.log('API rodando em http://localhost:3000'));

```
<br>
**Resumo final**

| Conceito              | Descrição                                  |
| --------------------- | ------------------------------------------ |
| **`req.body`**        | Dados enviados no corpo da requisição      |
| **Usado em**          | POST, PUT, PATCH, DELETE                   |
| **Necessita**         | `express.json()` ou `express.urlencoded()` |
| **Formato comum**     | JSON                                       |
| **Uso típico**        | Criar/atualizar dados                      |
| **Mais seguro**       | Dados não ficam expostos na URL            |
| **Melhores práticas** | Validar, tratar erros e converter tipos    |
<br>
