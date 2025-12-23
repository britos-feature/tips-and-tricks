# req.flash()

O **Flash Messenger** no **Express.js** é uma funcionalidade utilizada para exibir mensagens temporárias entre requisições HTTP. Ele é comumente usado para mostrar notificações ou feedbacks ao usuário após uma ação, como o envio de um formulário ou uma tentativa de login.


### **Como funciona o Flash Messenger?**

O **Flash Messenger** funciona armazenando mensagens na **sessão do usuário** e as removendo automaticamente assim que são exibidas. Isso significa que a mensagem só aparece uma única vez. 
Para implementar essa funcionalidade no **Express.js**, utilizamos o middleware **`connect-flash`**, que funciona em conjunto com **`express-session`.**


## **Implementação passo a passo**

### Instale os pacotes necessários

No seu projeto Express, instale os pacotes

```bash

npm install express express-session connect-flash

```

- `express`: framework web para Node.js.
- `express-session`: permite gerenciar sessões no Express.
- `connect-flash`: fornece suporte a mensagens flash.

### Sintaxe

```js
// flash messenger

// Criação da flash
req.flash("identificação", "mensagem");

// Utilização da flash
req.flash("identificação"); // aparece só uma vez após ser utilizada
```


### Configure o middleware no Express

```js
// main.js

const express = require("express");
const session = require("express-session");
const flash = require("connect-flash");

const app = express();

// Configuração da sessão
app.use(session({
    secret: "chave-secreta", 
    resave: false,
    saveUninitialized: true,
    cookie: { secure: false } // Defina como true se usar HTTPS
}));

// Middleware para flash messages
app.use(flash());

// Middleware para tornar as mensagens flash acessíveis no front-end
app.use((req, res, next) => {
    res.locals.success_msg = req.flash("success_msg");
    res.locals.error_msg = req.flash("error_msg");
    next();
});

// Configurar EJS como template engine (opcional)
app.set("view engine", "ejs");

```


### Criando rotas para mensagens flash

Agora podemos adicionar mensagens flash em diferentes rotas.
**Exemplo:** Mensagem de sucesso ao registrar um usuário

```js
// route.js

app.post("/register", (req, res) => {
    // Lógica para registrar o usuário...

    // Adiciona uma mensagem flash
    req.flash("success_msg", "Usuário registrado com sucesso!");
    
    // Redireciona para a página de login
    res.redirect("/login");
});

```

**Exemplo:** Mensagem de erro ao tentar logar

```js
// route.js

app.post("/login", (req, res) => {
    const { email, password } = req.body;

    if (email !== "admin@email.com" || password !== "1234") {
        req.flash("error_msg", "Credenciais inválidas!");
        return res.redirect("/login");
    }

    res.redirect("/dashboard");
});

```


### Exibindo mensagens flash no front-end (EJS)

Se você estiver usando **EJS**, pode exibir as mensagens flash assim

```html
<!-- index.ejs -->

<% if (success_msg.length > 0) { %>
    <div class="alert alert-success"><%= success_msg %></div>
<% } %>

<% if (error_msg.length > 0) { %>
    <div class="alert alert-danger"><%= error_msg %></div>
<% } %>

```

Caso esteja usando **Pug**, exemplo: **bootStrap**

```pug

if success_msg.length > 0
    div.alert.alert-success= success_msg

if error_msg.length > 0
    div.alert.alert-danger= error_msg

```


> **Pug** é uma **template engine** (motor de templates) para **Node.js** e **Express.js**, que permite escrever HTML de forma mais concisa e legível, eliminando a necessidade de fechar tags manualmente e reduzindo a quantidade de código.
> 
> Ele foi originalmente chamado de **Jade**, mas foi renomeado para **Pug** devido a questões de marca registrada.
> 
> **Principais Características do Pug**
> 
> **Sintaxe minimalista** (sem tags de fechamento).   
> **Indentação define a estrutura** (como no Python).  
> Suporte a variáveis, loops e condicionais.  
> **Integração fácil com Express.js**.

#### Pug é uma dependence (instalação necessária)

```bash

npm install pug

```


### Resumo

- **`connect-flash`** armazena mensagens temporárias na sessão.
- As mensagens são apagadas automaticamente após a exibição.
- Deve ser usado com **`express-session`** para funcionar corretamente.
- Útil para feedbacks de formulários, autenticação e ações do usuário.


