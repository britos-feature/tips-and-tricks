
## **O que é uma sessão no Express?**

Uma **sessão** é uma forma de armazenar dados temporários sobre o usuário entre diferentes requisições HTTP. Como o protocolo HTTP é sem estado (stateless), cada requisição feita ao servidor é independente da anterior. A sessão resolve esse problema armazenando informações no servidor e identificando cada usuário através de um **cookie de sessão**.

# req.session

 No Express.js **`req.session`** é um objeto que permite armazenar e gerenciar dados de sessão para usuários. Ele é usado principalmente para manter informações persistentes entre requisições HTTP, como autenticação de usuários, preferências e outros dados temporários.

Seu funcionamento é utilizando middleware para gerenciar sessões, e o mais comum é o **`express-session`**. Esse middleware armazena os dados da sessão no servidor e usa cookies no cliente para identificar sessões ativas.

> **`express-session`** é uma dependência do ExpressJS, por esse motivo é necessário instalar a mesma.


### **Particulariedades**

- **`express.session`** -> caso a propriedade **`saveUninitialized`** tiver ativar como **`true`** não será salvo nada no navegador ou no servidor. Só será salvo algo no navegador ou no servidor com inserido algum iten dentro do object  **`express.session`** 

**Exemplo:**
- Add iten **views** ao object **`express.session`**
	- **`express.session.views = 1`**  


### **Fluxo de funcionamento**

1. O usuário faz uma requisição ao servidor.
2. O Express gera um **ID de sessão único** e o armazena no servidor.
3. Um **cookie** (chamado `connect.sid` por padrão) contendo esse ID é enviado ao navegador do usuário.
4. Em requisições subsequentes, o navegador envia o **cookie de sessão** para o servidor.
5. O Express usa esse ID para recuperar os dados da sessão armazenados no servidor.

### **Principais Métodos e Propriedades**

| Propriedade                        | Descrição                                          |
| ---------------------------------- | -------------------------------------------------- |
| `req.session`                      | Objeto que armazena os dados da sessão do usuário. |
| `req.session.id`                   | ID único da sessão.                                |
| `req.session.destroy(callback)`    | Destroi a sessão do usuário.                       |
| `req.session.regenerate(callback)` | Cria uma nova sessão com um novo ID.               |
| `req.session.cookie`               | Acessa as configurações do cookie da sessão.       |

### Instalação e configuração **`express-session`**

- #### **Dependências**

```bash

npm install express-sessio

```


- #### Configuração do Middleware

```js
// main.js

const express = require("express");
const session = require("express-session");

const app = express();

app.use(session({
    // secret - Chave usada para assinar o cookie da sessão
    secret: "minha-chave-secreta", 
    // resave - Evita salvar sessão novamente se nada foi alterado    
    resave: false, 
    // saveUninitialezed -Salva sessões novas, mesmo se não forem modificadas
    saveUninitialized: true, 

    cookie: {
        // Definir como true apenas se estiver usando HTTPS
		secure: false, 
        // Tempo de expiração do cookie (10 minutos)
        maxAge: 1000 * 60 * 10 
    }
}));

app.get("/", (req, res) => {
    if (!req.session.visitas) {
        req.session.visitas = 1;
    } else {
        req.session.visitas++;
    }
    res.send(`Você visitou esta página ${req.session.visitas} vezes.`);
});

app.listen(3000, () => console.log("Servidor rodando na porta 3000"));

```


## **Configurações detalhadas do `express-session`**

| Configuração        | Descrição                                                               |
| ------------------- | ----------------------------------------------------------------------- |
| `secret`            | Uma string usada para assinar o cookie da sessão.                       |
| `resave`            | Se `false`, impede que a sessão seja salva novamente sem alterações.    |
| `saveUninitialized` | Se `true`, salva sessões vazias no armazenamento.                       |
| `cookie.secure`     | Se `true`, a sessão só será transmitida via HTTPS.                      |
| `cookie.maxAge`     | Tempo de expiração da sessão em milissegundos.                          |
| `cookie.httpOnly`   | Impede que JavaScript no navegador acesse o cookie (`true` por padrão). |

- #### Outro Middleware

```js
// main.js

const express = require("express");
const session = require("express-session");

const app = express();

app.use(session({
    secret: "chave-secreta", // Chave para assinar o cookie da sessão
    resave: false, // Evita salvar sessão se nada mudou
    saveUninitialized: true, // Salva sessões novas mesmo se vazias
    cookie: { secure: false } // true apenas se usar HTTPS
}));

app.get("/", (req, res) => {
    req.session.visits = (req.session.visits || 0) + 1;
    res.send(`Você visitou esta página ${req.session.visits} vezes.`);
});

app.listen(3000, () => console.log("Servidor rodando na porta 3000"));

```


### **Explicação do código acima**

1. O middleware **`express-session`** é configurado e adicionado ao app.
2. A chave **`secret`** protege os cookies da sessão.
3. O cookie da sessão pode ser configurado com diversas opções (como **`secure`** para HTTPS).
4. O número de visitas do usuário é salvo na sessão **(`req.session.visits`)**.


#### **Removendo a Sessão (Logout)**
Se você quiser limpar a sessão do usuário ao fazer logout, pode usar **`req.session.destroy()`**:

```js
// main.js

app.get("/logout", (req, res) => {
    req.session.destroy(err => {
        if (err) {
            return res.send("Erro ao encerrar a sessão.");
        }
        res.clearCookie("connect.sid"); // Remove o cookie do navegador
        res.send("Sessão encerrada com sucesso!");
    });
});

```

> Nota: **`connect.sid`** é o nome padrão do cookie de sessão, mas pode ser alterado na configuração.


#### **Armazenando Sessões em Bancos de Dados**

Por padrão, **`express-session`** armazena as sessões na memória do servidor, o que **não é recomendado** para produção, pois:

- O armazenamento em memória é **volátil** (perde dados ao reiniciar o servidor).
- Ele **não escala bem** para múltiplos servidores.

Soluções melhores incluem armazenar sessões em **Redis, MongoDB ou MySQL**. Para isso, podemos usar adaptadores como:

#### **Redis (`connect-redis`)**

```bash

npm install connect-redis ioredis

```

```js
// main.js

const RedisStore = require("connect-redis").default;
const Redis = require("ioredis");

const redisClient = new Redis();

app.use(session({
    store: new RedisStore({ client: redisClient }),
    secret: "minha-chave-secreta",
    resave: false,
    saveUninitialized: false,
    cookie: { secure: false, maxAge: 1000 * 60 * 30 } // 30 minutos
}));

```


#### MongoDB **(connect-mongo)**

```bash

npm install connect-mongo mongoose

```

```js
// main.js

const MongoStore = require("connect-mongo");
const mongoose = require("mongoose");

mongoose.connect("mongodb://localhost/sessoes");

app.use(session({
    store: MongoStore.create({ mongoUrl: "mongodb://localhost/sessoes" }),
    secret: "minha-chave-secreta",
    resave: false,
    saveUninitialized: false,
    cookie: { secure: false, maxAge: 1000 * 60 * 30 } // 30 minutos
}));

```


#### MySQL (**express-mysql-session**)

```bash

npm install express-mysql-session mysql2

```

```js
// main.js

const MySQLStore = require("express-mysql-session")(session);
const mysql = require("mysql2");

const connection = mysql.createConnection({
    host: "localhost",
    user: "root",
    password: "senha",
    database: "sessoes_db"
});

const sessionStore = new MySQLStore({}, connection);

app.use(session({
    store: sessionStore,
    secret: "minha-chave-secreta",
    resave: false,
    saveUninitialized: false,
    cookie: { secure: false, maxAge: 1000 * 60 * 30 } // 30 minutos
}));

```


## **Dicas de Segurança**

1. **Use `secure: true` em produção** (com HTTPS) para proteger o cookie da sessão.
2. **Defina `httpOnly: true`** para evitar que scripts maliciosos acessem o cookie.
3. **Armazene sessões em Redis ou bancos de dados** para melhor desempenho.
4. **Habilite expiração de sessões (`maxAge`)** para evitar que usuários fiquem logados indefinidamente.


## **Resumo**

- **`req.session`** armazena dados do usuário no servidor.
- O **`express-session`** usa um cookie para identificar sessões.
- As sessões podem ser armazenadas em memória, Redis, MongoDB ou MySQL.
- Use **`req.session.destroy()`** para encerrar uma sessão.
- Configure **`secure: true`** e **`httpOnly: true`** para segurança.

---
---
