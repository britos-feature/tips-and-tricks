# EJS (Embedded JavaScript)

### O que é EJS ?

EJS (Embedded JavaScript) é uma linguagem de template que permite inserir código JavaScript dentro de arquivos HTML para renderizar páginas dinâmicas. Ele é amplamente usado em aplicações Node.js, especialmente com o framework Express.js.

Ele funciona substituindo placeholders (marcadores) dentro do HTML por valores reais antes de enviar a página ao cliente.

### **Tags**

- **`<%`** = Tag 'Scriptlet', para controle-fluxo, sem saída
- **`<%_`** = Tag ‘Whitespace Slurping’ Scriptlet, tira todo o espaço em branco antes dele
- **`<%=`** = Saídas o valor no modelo (HTML escape)
- **`<%-`** = Saída o valor não descapado no modelo
- **`<%#`** = Tag de comentário, sem execução, sem saída
- **`<%%`** = Saídas um "?%" literal
- **`%>`** = Tag de encerramento simples
- **`-%>`** = Tag Trim-mode ('newline slurp'', aparas seguindo a nova linha
- **`_%>`** = Tag de final 'Whitespace Slurping', remove todo o espaço em branco depois dele

### **Como instalar e configurar EJS**

```bash

npm install ejs

```

### **Configurar o Express.js para usar EJS**

```js
// main.js

const express = require("express");
const app = express();

// Configurar EJS como engine de visualização
app.set("view engine", "ejs");

// Definir o diretório onde estão os templates
app.set("views", p__dirname + "/views");
```

### **Injetando dados via EJS \***

- ##### **INJEÇÃO especificamente para ROTA ATUAL**

```js
// main.js = NodeJS

const express = require("express");
const path = require("node:path");
const app = express();

app.set("view engine", "ejs");
app.set("views", path.resolve(__dirname, "views"));

// Tem acesso apenas apenas **NESTA ROTA** aos objects declarados
app.get("/", (req, res) => {
  res.render("index", {
    title: "Esse será meu titulo",
    numberArray: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
  });
});
```

- ##### **INJEÇÃO em todas as ROTAS (via Middleware)**

```js
// main.js

const express = require("express");
const path = require("node:path");
const app = express();

// disponibilizando para todas as rotas a variavel locals.user
app.use((req, res, next) => {
  res.locals.user = { user: "User", lasname: "LastUser", age: 49 };
  next();
});

// Nesta ROTA tem acesso a variável locals.user e aos objects declarado da ROTA
app.get("/", (req, res) => {
  res.render("index", { tilte: "Este será meu titulo" });
});

// TEM Acesso a variável locals.user
app.get("/login", (req, res) => {
  res.render("login");
});
```

#### **Usando `res.locals` e variavel especificada na ROTA no arquivo EJS**

Agora, você pode acessar essas variáveis dentro dos arquivos EJS, sem precisar passá-las explicitamente ou até podendo através de `res.render()`.

```html
<!-- index.ejs -->

<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%= title %></title>
    <!-- variável da ROTA -->
  </head>
  <body>
    <header>
      <h1>Bem-vindo ao <%= locals.user.user %></h1>
      <!-- variável res.locals -->
      <h2>Sua idade é: <%= locals.user.age %></h2>
      <!-- variável res.locals -->
      <h3>Esse o nome do site: <%= title %></h3>
      <!-- variável da ROTA -->
    </header>
    <main>
      <h2>Página Inicial</h2>
      <p>Conteúdo da página inicial...</p>
    </main>
  </body>
</html>
```
