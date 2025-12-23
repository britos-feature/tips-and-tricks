**Middleware** em JavaScript, especialmente no contexto de frameworks como **Express.js** (muito usado com Node.js), é uma **função que tem acesso ao objeto `req` (request), ao objeto `res` (response) e à função `next()`** no ciclo de requisição-resposta de uma aplicação web.

> Um **middleware** é uma função que **intercepta** a requisição **antes** que ela chegue à rota final, podendo fazer verificações, modificações ou até encerrar a resposta.

#### Estrutura básica de um Middleware

```js
function meuMiddleware(req, res, next) {
  console.log('Middleware executado!');
  next(); // passa para o próximo middleware ou rota
}
```


#### Utilizando o Middleware

```js
const express = require('express');
const app = express();

// Middleware global
app.use((req, res, next) => {
  console.log(`Requisição recebida em: ${req.url}`);
  next(); // Continua para o próximo middleware ou rota
});

// Rota
app.get('/', (req, res) => {
  res.send('Olá, mundo!');
});
```


#### Tipos de middleware:

##### **Globais:** afetam todas as rotas.

```js
const express = require('express');
const app = express();
const myMiddleware = require ('middleware');  // middleware.js

app.use(myMiddleware);
app.get('/', (req, res) => {});
```

#### **De rota:** aplicados apenas a rotas específicas.

```js
const express = require('express');
const app = express();

app.get('/', middleware, (req, res) => {});
```

 > Ainda tem os **de erro** que lidam com **erros** (têm 4 parâmetros: `err, req, res, next`),
 >  e os de **terceiros:** como: `body-parser`, `cors`, etc.


**Resumo:**
- Middleware é uma função intermediária entre o pedido do cliente e a resposta do servidor.
- Pode **modificar o `req` ou `res`**, **encerrar a resposta** ou **passar para o próximo passo com `next()`**.
- Muito usado para **autenticação, logs, tratamento de erros, parsing de dados, etc.**


