O **Express.js** é um **framework minimalista e flexível** para o **Node.js** usado para criar **servidores web e APIs**.  
Ele simplifica o uso do módulo `http` nativo do Node e adiciona recursos como:

- Gerenciamento de **rotas** (URLs diferentes, respostas diferentes)
- Suporte a **requisições e respostas HTTP** mais fáceis
- Uso de **middlewares** (funções intermediárias)
- Suporte a **templates** e **arquivos estáticos**
- Facilita a criação de **APIs RESTful**

**Recursos avançados**

| Recurso                | Descrição                                        |
| ---------------------- | ------------------------------------------------ |
| **Routers**            | Separar rotas em módulos (`express.Router()`)    |
| **Template Engines**   | Renderizar HTML dinâmico (EJS, Pug, Handlebars)  |
| **Middleware externo** | Usar pacotes como `cors`, `morgan`, `helmet`     |
| **APIs RESTful**       | Criar rotas que retornam JSON                    |
| **Autenticação**       | Usar bibliotecas como `passport`, `jsonwebtoken` |
<br>
### Começar com Express (necessário NodeJS)

- **Instalação do Express
	- Crie um projeto Node.js (caso ainda não tenha)

```Bash
mkdir meu-projeto
cd meu-projeto
npm init -y
```  
<br>
- **Instale o Express**

```Bash
npm install express
```  
<br>
- **Criando um Servidor Básico**

```js
// app.js

const express = require('express');
const app = express();

// Rota principal
app.get('/', (req, res) => {
  res.send('Olá, mundo!');
});

// Servidor ouvindo na porta 3000
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```
<br>
- **Iniciar  Servidor**
	- Via Bash comando direto

```sh

node app.js # node direct

```
<br>
- **Parar Servidor**

```sh

ctrl +  alt + m

```
<br>
### Rotas básico - "HTML"

**Rotas (routes)** em **HTML** é o conceito de **roteamento (routing)** — algo que não é feito diretamente pelo HTML, mas sim por **servidores web** ou **frameworks JavaScript** (como Express no Node.js, React Router, etc.).

Uma **rota (route)** é basicamente o **caminho (URL)** que o navegador acessa para exibir uma determinada **página** ou **recurso**.

**Exemplo:**

|URL (rota)|Recurso/Página|
|---|---|
|`/`|Página inicial (index.html)|
|`/sobre`|Página sobre (sobre.html)|
|`/contato`|Página de contato (contato.html)|
<br>
### Express Route
 Ao criamos um **servidor web( Express )**, podemos definir **rotas no backend**, ou seja, o servidor decide **qual página HTML enviar** para cada rota.

**Exemplo:**

```js
// app.js

import express from 'express';
const app = express();

// Rota principal
app.get('/', (req, res) => {
  res.sendFile('index.html', { root: '.' });
});

// Rota "sobre"
app.get('/sobre', (req, res) => {
  res.sendFile('sobre.html', { root: '.' });
});

// Rota "contato"
app.get('/contato', (req, res) => {
  res.sendFile('contato.html', { root: '.' });
});

app.listen(3000, () => console.log('Servidor rodando em http://localhost:3000'));

```

>  Aqui, o **servidor** define as rotas e decide qual arquivo HTML enviar ao navegador.
<br>
