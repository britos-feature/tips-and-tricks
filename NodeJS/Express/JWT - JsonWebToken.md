
# Apresentação do JWT

**JWT (JSON Web Token)** é um **formato padronizado** de token baseado em JSON que permite transmitir informações **entre duas partes de forma segura e compacta**.  
Ele é **autocontido**, ou seja, carrega em si mesmo todas as informações necessárias sobre o usuário ou sessão.

**Explicação:** Pense nele como um “**cartão de acesso digital**”:

- Ele identifica quem você é (**payload**)
- É assinado para provar que é autêntico (**signature**)
- Pode ter validade (**expiração**)

### Estrutura detalhada de um JWT

Um **Token** sempre tem **3**(três) partes:  _**`HEADER.PAYLOAD.SIGNATURE`**_
Cada parte é codificada em **Base64Url** e separada por pontos.

1. **Header**
	1. Contém metadados sobre o token — principalmente:
			- o **algoritmo de assinatura** (`HS256`, `RS256`, etc.)
			- o **tipo de token** (`JWT`)

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

```nginx

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 # resultado codificado do Header 

```

2. **PAYLOAD** (Corpo de dados)
	1. Contém os **dados (claims)** — informações sobre o usuário ou sessão. 
		Há três tipos de _claims_:
		- **Registered claims** -> Campos padrão definidos pelo **JWT** -> `iss`, `sub`, `exp`, `iat`
		- **Public claims** -> Campos personalizados e compartilháveis -> `email`, `role`, `name`
		- **Private claims** -> Campos internos definidos pela aplicação -> `user_id`, `permissions`

```json
{
  "sub": "1234567890",
  "name": "Alex Brito",
  "email": "alex@email.com",
  "role": "admin",
  "iat": 1735849200,
  "exp": 1735856400
}
```

```nginx

eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkFsZXggQnJpdG8iLCJlbWFpbCI6ImFsZXhAZW1haWwuY29tIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzM1ODQ5MjAwLCJleHAiOjE3MzU4NTY0MDB9
# resultado codificado do Playload

```
<br>
3. **Signature** (Assinatura)
	- É a parte que **garante que o token não foi adulterado**.
		- Ela é criada combinando:

```js

// Criada combinando.
signature = HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

> O servidor usa uma **chave secreta** (ou par de chaves pública/privada) para gerar essa assinatura.

```nginx

AbCdEf12345GhIjKlMnOpQrStUvWxYz # resultado codificado da Signature

```


### Resultado completo de um **JWT**:

``` js

HEADER.PAYLOAD.SIGNATURE

```
<br>

---
---

## Funcionamento na prática:

1. **User faz login**
	Supondo em uma aplicação com **login e rotas protegidas**:

```http

POST /login
Content-Type: application/json

{
  "email": "alex@email.com",
  "password": "123456"
}

```
<br>
2. **Servidor valida credenciais**
	Se estiver tudo correto, ele gera um **JWT**

```json

import jwt from "jsonwebtoken";

const token = jwt.sign(
  { id: user.id, name: user.name, role: user.role },
  process.env.JWT_SECRET,
  { expiresIn: '1h' } // expira em 1 hora
);

```
<br>
3. **Servidor envia** **_Token_**

```json

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI..."
}

```

> <big><b><span style="color: red">Necessário cliente armazena o token em:</span> (localStorage, sessionStorage, ou cookie).</b></big>
<br>
4. **Cliente realiza request(requisições) protegidas

```http

GET /users
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI...

```
<br>
5. **Servidor valid Token**

```js

import jwt from "jsonwebtoken";

const decoded = jwt.verify(token, process.env.JWT_SECRET);
console.log(decoded); 
// { id: 1, name: 'Alex Brito', role: 'admin', iat:..., exp:... }

```

> Se for válido ➜ acesso liberado  
> Se for inválido/expirado ➜ 401 Unauthorized
<br>
### **Vantagens reais do JWT**

✅ **Escalável** — não precisa guardar sessões no servidor.  
✅ **Desacoplado** — ideal para APIs RESTful e microserviços.  
✅ **Seguro** — a assinatura impede alterações maliciosas.  
✅ **Prático** — pode incluir qualquer dado no payload.

---

### Limitações e Cuidados

❌ **Não criptografa** o conteúdo — apenas codifica (Base64).  /Alguém pode ler o payload, mesmo que não consiga alterar.
❌ **Tokens não podem ser revogados facilmente.**  / Você pode resolver isso com _blacklists_ ou _token rotation_.
❌ **Evite guardar dados sensíveis** (senhas, CPF, etc.) dentro do JWT.
❌ **Use sempre HTTPS**, para que o token não seja interceptado.


---
---

## Exemplo completo utilizando com _**JWT em Node.js + Express**_

**inclui**:
- login e geração do token
- middleware para proteger rotas
- validação do token
- explicações linha por linha

### 1. Instalação de dependências

```sh

npm init -y
npm install express jsonwebtoken dotenv

```

### 2. Criação do arquivo .env (dotenv)

> Aqui guardaremos a chave secreta usada para assinar os tokens.

```env

JWT_SECRET=meusegredosuperseguro
JWT_EXPIRES_IN=1h
PORT=3000

```

### 3. Criação do arquivo **_server.js_**

> Esse arquivo inicializa o servidor e carrega as rotas.

```js

import express from "express";
import dotenv from "dotenv";
import routes from "./src/routes.js";

dotenv.config();

const app = express();
app.use(express.json());
app.use(routes);

app.listen(process.env.PORT, () => {
  console.log(`Server running on port ${process.env.PORT}`);
});

```

### 4. Controller de autenticação

`src/controllers/authController.js`

> Esse controller simula um login e gera um **token JWT** válido por 1 hora.

```js

import jwt from "jsonwebtoken";
import dotenv from "dotenv";

dotenv.config();

class AuthController {
  async login(req, res) {
    try {
      const { email, password } = req.body;

      // Simulação de usuário (em um app real, você buscaria no banco)
      const user = {
        id: 1,
        name: "Alex Brito",
        email: "alex@email.com",
        password: "123456", // nunca armazenar senha assim!
      };

      // Verifica credenciais
      if (email !== user.email || password !== user.password) {
        return res.status(401).json({ error: "Invalid credentials" });
      }

      // Cria o token
      const token = jwt.sign(
        { id: user.id, name: user.name, email: user.email },
        process.env.JWT_SECRET,
        { expiresIn: process.env.JWT_EXPIRES_IN }
      );

      return res.json({
        message: "Login successful!",
        token,
      });
    } catch (err) {
      return res.status(500).json({ error: "Server error" });
    }
  }
}

export default new AuthController();

```

### 5. Middleware de proteção

`src/middlewares/loginRequired.js`

**Esse middleware:**
	- Lê o token do cabeçalho `Authorization: Bearer <token>`
	- Valida sua assinatura
	- Se for válido, libera o acesso à rota
	- Caso contrário, retorna `401 Unauthorized`

```js

import jwt from "jsonwebtoken";
import dotenv from "dotenv";

dotenv.config();

export default (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader) {
    return res.status(401).json({ error: "Token not provided" });
  }

  const [, token] = authHeader.split(" "); // "Bearer token"

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    req.userId = decoded.id;
    req.userEmail = decoded.email;

    return next();
  } catch (err) {
    return res.status(401).json({ error: "Invalid or expired token" });
  }
};

```

### 6. Controller de user

`src/controllers/userController.js`

> Aqui apenas retornamos dados de quem acessou (extraídos do token).

```js

class UserController {
  async index(req, res) {
    return res.json({
      message: "Welcome to the protected route!",
      user: {
        id: req.userId,
        email: req.userEmail,
      },
    });
  }
}

export default new UserController();

```

### 7. Rotas

``src/routes.js

```js

import { Router } from "express";
import AuthController from "./controllers/AuthController.js";
import UserController from "./controllers/UserController.js";
import loginRequired from "./middlewares/loginRequired.js";

const router = Router();

// 🔓 Rota pública
router.post("/login", AuthController.login);

// 🔒 Rota protegida
router.get("/user", loginRequired, UserController.index);

export default router;

```


---

## Testando _**app exemplo**_

### _1. **Login route public**_

```sh

POST http://localhost:3000/login
Content-Type: application/json

{
  "email": "alex@email.com",
  "password": "123456"
}

```

### _**Response**_

```json

{
  "message": "Login successful!",
  "token": "eyJhbGciOiJIUzI1NiIsInR5c..."
}

```


### _2. Protected route_

> Envie o token no cabeçalho

```sh

GET http://localhost:3000/user
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5c...

```

### _**Response**_

```json
{
  "message": "Welcome to the protected route!",
  "user": {
    "id": 1,
    "email": "alex@email.com"
  }
}

```

**Se o token for inválido ou expirado:**

```json

{
  "error": "Invalid or expired token"
}

```


---

### Resumo do fluxo

1️ Usuário faz login → recebe JWT  
2️ Cliente guarda o token  
3️ Cliente envia o token em cada requisição  
4️ Middleware valida o token → libera ou bloqueia  
5️ Nenhuma sessão fica guardada no servidor

