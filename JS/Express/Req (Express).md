
# Express (req, res)

No contexto do **Node.js** e do framework **Express.js**, os objetos **`req` (request)** e **`res` (response)** são fundamentais para lidar com requisições HTTP e enviar respostas. Eles são passados automaticamente como parâmetros para as funções de middleware e rotas.
## **`req`** (request)

O objeto `req` contém informações sobre a requisição feita pelo cliente, como parâmetros, corpo da requisição, cabeçalhos, método HTTP, entre outros:

- **Cabeçalhos HTTP** (headers)
- **Parâmetros de URL** (query params e route params)
- **Dados do corpo da requisição** (body)
- **Cookies**
- **Método HTTP** (GET, POST, PUT, DELETE etc.)
- **Endereço IP do cliente**
- **Protocolo e URL de referência**

### **Principais propriedades do `req`:**

- **`req.body`** → Contém os dados enviados no corpo da requisição (precisa do middleware `express.json()` para JSON).
- **`req.params`** → Contém os parâmetros de rota definidos com `:`.
- **`req.query`** → Contém os parâmetros da URL (`?chave=valor`).
- **`req.headers`** → Contém os cabeçalhos da requisição.
- **`req.method`** → Retorna o método HTTP usado (`GET`, `POST`, etc.).
- **`req.originalUrl`** -> Retorna a URL da solicitação original.
- **`req.url`** → Retorna a URL solicitada.
- **`req.path`** -> Retorna o caminho da Url (sem domínio e sem query string)
- **`req.cookies`** -> Retorna os cookies enviados pelo cliente (se `cookie-parser` estiver ativo)
- **`req.ip`** -> Retorna o endereçõ IP do clientee

- **`req.session`**


### **`req.body`**

Em **Express.js**, `req.body` é um objeto que contém os dados do corpo da requisição (request body). Ele é usado para acessar informações enviadas pelo cliente (por exemplo, em uma requisição **POST**, **PUT** ou **PATCH**).

> O **`req.body`** está disponível apenas se um **middleware** adequado for usado para processar os dados da requisição. O **Express** não processa automaticamente o corpo da requisição, então você precisa adicionar middlewares para interpretar diferentes formatos, como: **`express.json()`, `express.urlencoded()`**


#### Exemplos:

**`express.json()`**

```js
// main.js

const express = require("express");
const app = express();

app.use(express.json()); // Middleware para processar JSON

app.post("/dados", (req, res) => {
  console.log(req.body); // Exibe o corpo da requisição no console
  res.send("Dados recebidos!");
});

app.listen(3000, () => console.log("Servidor rodando na porta 3000"));

```

#### Arquivo JSON enviado 

```json
// Normalmete dados passado via formulário

{
	"name": "Alex",
	"age": 49
}
```

#### Retorno do **`req.body`**

```bash
# return console

{ name: "Alex", age: 49 }

```

---
---

### **`req.params`**

O `req.params` é um objeto que contém os **parâmetros dinâmicos** da URL em rotas definidas com **placeholders (`:`)**. Ele é útil para capturar valores passados diretamente na **rota**, como identificadores (`id`), nomes ou categorias.

#### Funcionamento dos **``params`**

No **Express.js**, quando você define uma rota com **`:`** antes do nome de um parâmetro, ele se torna dinâmico. O valor enviado pelo usuário nessa posição da URL será acessado através do **`req.params`**.

#### Estrutura básica:

```js
// main.js

const express = require("express");
const app = express();

// definição para ROTES (params obrigatório)
app.get("/user/:id", (req, res) => {
	const userID = req.params.id;
	res.send(`User ID is ${userID}`);
});

app.listen(3000, () => {
	console.log('Server is running on port 3000');
	});

/* Quando o servidor Express recebe uma requisição do tipo GET para uma URL como .../user/123, o req.params receberá um object {id: 123}.

OBS. Nesse tipo de definição acima a passagem do parâmetro é OBRIGATÓRIA
********************************************************************************/


// definição para ROTES (params NÃO obrigatório)
app.get("/user/:id?", (req, res) => {
	const userID = req.params.id;
	res.send(`User ID is ${userID}`);
});

/* OBS. Nesse tipo de definição acima a passagem do parâmetro é OBRIGATÓRIA

Nessa requisição do tipo GET para uma URL como .../user/? o parâmetro é OPCIONAL 
No caso do parâmetro ser passado na URL o req.params retornará o object {id: valor}.
Caso o parâmetro NÂO ser passado o mesmo retornará um object empty (vazio)

/ = definição para ROTE
: = definição para PARÂMETROS
? = definição para OPCIONAL ou inicio de queryString
& = separação de params para queryString

********************************************************************************/


// definição para ROTES juntamente como params (params NÃO obrigatório)
app.get("/user?:id?", (req, res) => {
	const userID = req.params.id;
	res.send(`User ID is ${userID}`);

/*
Nessa requisição do tipo GET para uma URL como .../user/? o parâmetro é OPCIONAL 
No caso do parâmetro ser passado na URL o req.params retornará o object {id: valor}.
Caso o parâmetro NÂO ser passado o mesmo retornará um object (id : undefined)
********************************************************************************/
});
```

---
---

### **`req.query`**

Em **Express.js**, a propriedade **`req.query`** é um objeto que contém os parâmetros da **query string** enviados em uma requisição HTTP **GET**. Esses parâmetros são extraídos da URL da requisição e disponibilizados como pares **chave-valor** no objeto **`req.query`.**

#### Características do `req.query`

1. **Sempre retorna um objeto** -> Mesmo que a query string esteja vazia, `req.query` será um **objeto vazio (`{}`)**.

2. **Valores sempre são strings** -> Os valores são sempre tratados como **strings**, então, se precisar de um número, você deve convertê-lo.

3. **Suporte a múltiplos valores para a mesma chave** -> Se um parâmetro for enviado mais de uma vez, **`req.query`** pode armazená-los como **array**.

4. **Uso de query strings aninhadas** -> Se for necessário enviar objetos aninhados na query string, podemos usar bibliotecas como [`qs`](https://www.npmjs.com/package/qs)


#### Estrutura básica

Quando um cliente faz uma requisição com parâmetros na query string, o Express os extrai automaticamente e os disponibiliza em **`req.query`**.

```pgsql

GET /users?name=João&age=25

```

**Aqui, os parâmetros da query string são:**
- name = "João"
- age = "25"

#### Exemplo code

```js
main.js

const express = require('express');
const app = express();

app.get('/users', (req, res) => {
    console.log(req.query); // { name: 'João', age: '25' }
    res.send(`Nome: ${req.query.name}, Idade: ${req.query.age}`);
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

/*
Quando acessamos `http://localhost:3000/users?name=João&age=25`, a propriedade req.query retorna um object com os parâmetros da URL {"name": "João, "age": "25"}
*/

// Converte string para number
const age = parseInt(req.query.age, 10);
// req.query.age = valor da string
// 10 = valor base radix para conversão


```


#### Uso de query strings aninhadas (URL)

```js
// main.js

const qs = require('qs');

app.get('/test', (req, res) => {
    console.log(req.query); // { user: { name: 'Maria', age: '30' } }
    res.send(req.query);
});

/*
Quando acessamos `http://localhost:300/user?user[name]=Maria&user[age]=30`
req.query = { user: {name: "Maria", age: 30}}
```


#### Uso de query string juntamente com params

```js
// main.js

const express = require("express");
const app = express();

app.get("/product?/:name", (req, res) => {
	console.log(req.params);
	console.log(req.query);
	res.send(req.params);
});

/*
Quando acessamos `http://localhost:3000/product/Cel?&model=A25&color=blue&memory=8g`, a propriedade req.params retorna um object com o parâmetro da URL {"name": "cel"} e req.query retorna um object com os parâmetros da URL {"model": "A25, "color": "blue", "memory": "8g"}
*/

app.listen(3000, () => console.log("Servidor rodando na porta 3000"));
```

#### Diferença entre `req.query` e `req.params`

- `req.query` → Captura **parâmetros na query string** (`?chave=valor`).
- `req.params` → Captura **parâmetros na URL definida na rota**.


#### Resumo

A propriedade `req.query` do Express.js é uma maneira prática e fácil de acessar os parâmetros da query string enviados pelo cliente. Como os valores são sempre strings, é importante converter os tipos quando necessário. Para dados mais complexos, bibliotecas como `qs` podem ajudar.

---
---


### **`req.headers`**

No Express (um framework para Node.js), a propriedade **`req.headers`** representa os cabeçalhos HTTP da requisição recebida pelo servidor. Essa propriedade é um objeto contendo pares chave-valor, onde as chaves são os nomes dos cabeçalhos e os valores são seus respectivos conteúdos.

#### **O que são os cabeçalhos HTTP?**

Os **cabeçalhos HTTP (HTTP headers)** são metadados enviados junto a uma requisição ou resposta HTTP. Eles fornecem informações adicionais, como:

- Tipo de conteúdo (`Content-Type`)
- Autenticação (`Authorization`)
- Controle de cache (`Cache-Control`)
- Origem da requisição (`Origin`, `Referer`)
- Entre outros


#### **Acessando `req.headers` no Express**

Em um aplicativo Express, **`req.headers`** pode ser acessado dentro de um middleware ou rota.

#### Estrutua básica

```js
// main.js

const express = require('express');
const app = express();

app.get('/', (req, res) => {
    console.log(req.headers); // Exibe todos os cabeçalhos da requisição
    res.send('Verifique o console para ver os headers');
});

app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});

```


#### Requisição curl -H "Custom-Header: TestValue" http://localhost:3000

```bash

curl -H "Custom-Header: TestValue" http://localhost:3000


# return
{
  "host": "localhost:3000",
  "connection": "keep-alive",
  "custom-header": "TestValue",
  "user-agent": "curl/7.64.1",
  "accept": "*/*"
}

```

> **Observação:** Os nomes das chaves dos cabeçalhos são convertidos para **minúsculas** no objeto `req.headers`.


#### **Acessando Cabeçalhos Específicos**

Para acessar um cabeçalho específico, basta referenciar a chave correspondente

```js
// main.js

app.get('/', (req, res) => {
    const userAgent = req.headers['user-agent'];  // Obtém o User-Agent
    res.send(`Seu User-Agent é: ${userAgent}`);
});

```


#### **Cabeçalhos Comuns em uma Requisição HTTP**

Aqui estão alguns dos cabeçalhos mais comuns que podem ser encontrados em **`req.headers`**

|Cabeçalho|Descrição|
|---|---|
|`host`|Indica o domínio e porta do servidor requisitado|
|`user-agent`|Identifica o cliente (navegador ou ferramenta) que fez a requisição|
|`content-type`|Indica o tipo do corpo da requisição (ex: `application/json`)|
|`authorization`|Contém informações de autenticação, como tokens JWT|
|`accept`|Indica os tipos de conteúdo que o cliente pode aceitar|
|`referer`|Indica a página de origem que fez a requisição|
|`origin`|Indica a origem da requisição (usado em CORS)|

## **Verificando se um Cabeçalho Existe**

Se precisar verificar se um cabeçalho específico foi enviado

```js
// main.js

app.get('/', (req, res) => {
    if (req.headers['authorization']) {
        res.send('Cabeçalho de Authorization encontrado!');
    } else {
        res.status(400).send('Cabeçalho de Authorization não encontrado!');
    }
});

```


#### **Definindo Cabeçalhos na Resposta (`res.setHeader`)**

Embora **`req.headers`** seja somente leitura (representando os cabeçalhos da requisição), podemos definir cabeçalhos na resposta usando **`res.setHeader`**

```js
// main.js

app.get('/', (req, res) => {
    res.setHeader('X-Powered-By', 'Express');
    res.send('Cabeçalhos definidos!');
});

```


#### **Resumo**

- **`req.headers`** contém todos os cabeçalhos da requisição HTTP como um objeto JavaScript.
- Os nomes das chaves são sempre em **minúsculas**.
- É possível acessar cabeçalhos individuais usando **`req.headers['nome-do-cabecalho']`**.
- Cabeçalhos são importantes para segurança, autenticação, CORS e formatação de requisições.

---
---

### **`req.method`**

O **`req.method`** é uma propriedade do objeto **`req`** **(request)** no Express.js que representa o método HTTP da requisição recebida pelo servidor. Esse valor pode ser usado para verificar se a requisição foi feita utilizando **`GET`, `POST`, `PUT`, `DELETE`** ou outro método HTTP.

#### **Funcionamento do `req.method`**

Quando um cliente (como o navegador ou um serviço externo) faz uma requisição para o servidor Express, ele pode usar diferentes métodos HTTP. O Express disponibiliza o objeto `req` dentro do **middleware** e das rotas para acessar informações sobre a requisição, incluindo o método HTTP.

#### Estrutura básica

```js
// main.js

const express = require('express');
const app = express();

app.use((req, res, next) => {
    console.log(`Método da requisição: ${req.method}`);
    next(); // Passa o controle para o próximo middleware
});

app.get('/', (req, res) => {
    res.send('Esta é uma requisição GET');
});

app.post('/', (req, res) => {
    res.send('Esta é uma requisição POST');
});

app.listen(3000, () => {
    console.log('Servidor rodando na porta 3000');
});

```


#### **Explicação do código**

1. **Middleware global**: Antes de qualquer rota ser processada, o middleware captura e imprime o método da requisição com `req.method`.
2. **Rota GET (`app.get()`)**: Retorna uma resposta quando uma requisição `GET` é feita para `/`.
3. **Rota POST (`app.post()`)**: Retorna uma resposta quando uma requisição `POST` é feita para `/`.


**Se você rodar o servidor e fizer uma requisição `GET` para `http://localhost:3000/`, o console mostrará**

```text

Método da requisição: GET

```

**Se fizer uma requisição `POST`, mostrará**

```text

Método da requisição: POST

```


#### **Usando `req.method` para tratar múltiplos métodos**

Em alguns casos, pode ser útil tratar múltiplos métodos HTTP dentro de um único manipulador de rota. Podemos usar **`req.method`** para diferenciar as ações.

```js
// main.js

app.all('/usuarios', (req, res) => {
    if (req.method === 'GET') {
        res.send('Listando usuários');
    } else if (req.method === 'POST') {
        res.send('Criando um novo usuário');
    } else {
        res.status(405).send(`Método ${req.method} não permitido`);
    }
});

```

#### Explicação:

- Se um `GET` for enviado para `/usuarios`, o servidor responde com **"Listando usuários"**.
- Se um `POST` for enviado para `/usuarios`, ele responde com **"Criando um novo usuário"**.
- Se outro método (como `PUT` ou `DELETE`) for enviado, o servidor responde com **405 - Método não permitido**.


#### ### **Logs e monitoramento**

Registrar o método HTTP pode ser útil para depuração ou análise de tráfego

```js
// main.js

app.use((req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
});

```


#### ### **Autenticação baseada no método**

Um middleware pode permitir ou bloquear métodos específicos para determinadas rotas

```js
// main.js

app.use('/admin', (req, res, next) => {
    if (req.method === 'GET') {
        return res.status(403).send('Acesso negado para GET nesta rota.');
    }
    next();
});

```


#### **API RESTful com um único endpoint**

Podemos criar um CRUD simples para um recurso, como `produtos`, tratando cada método dentro da mesma rota

```js
//main.js

app.all('/produto', (req, res) => {
    switch (req.method) {
        case 'GET':
            res.send('Buscando produto');
            break;
        case 'POST':
            res.send('Criando produto');
            break;
        case 'PUT':
            res.send('Atualizando produto');
            break;
        case 'DELETE':
            res.send('Removendo produto');
            break;
        default:
            res.status(405).send('Método não permitido');
    }
});

```


#### Resumo

- **`req.method`** é uma propriedade útil para identificar o tipo de requisição HTTP recebida pelo servidor Express.

- **Alternativa**: O Express já fornece métodos como `app.get()`, `app.post()`, `app.put()` e `app.delete()`, mas `req.method` pode ser útil para controle centralizado.

---
---


### **`req.originalUrl`**

No Express.js, a propriedade `req.originalUrl` é usada para obter a URL completa da requisição original feita pelo cliente, incluindo o caminho e os parâmetros de consulta (query string), mas sem o domínio.

Ela é útil quando você precisa acessar a URL completa sem qualquer modificação feita por middlewares.

A propriedade **`req.originalUrl`** contém a URL exata que o cliente usou para acessar o servidor, desde a raiz da aplicação. Isso inclui:

- O **path** (caminho da rota)
- A **query string** (parâmetros de consulta)

#### Estrutura básica

```js
// main.js

const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('URL original:', req.originalUrl);
  next();
});

app.get('/produto/:id', (req, res) => {
  res.send(`Você acessou a URL: ${req.originalUrl}`);
});

app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});


/*
Se um cliente acessar -> http://localhost:3000/produto/123?cor=azul&tamanho=M

A saída no console será -> URL original: /produto/123?cor=azul&tamanho=M`

E a resposta da API será -> `Você acessou a URL: /produto/123?cor=azul&tamanho=M`
*/

```


#### Diferença entre `req.url` e `req.originalUrl`

#### `req.url`

- Contém apenas a **parte da URL após o domínio**, incluindo query strings.
- Se um middleware modificar a requisição, **`req.url`** pode refletir essa alteração.

#### `req.originalUrl`

- Sempre mantém a **URL original** usada pelo cliente.
- Mesmo que middlewares alterem **`req.url`, `req.originalUrl`** continua intacta.


#### Exemplo com Middleware que altera `req.url`

```js
// main.js

const express = require('express');
const app = express();

app.use('/api', (req, res, next) => {
  console.log('req.url antes:', req.url);
  console.log('req.originalUrl:', req.originalUrl);
  
  req.url = '/modificado'; // Altera apenas req.url
  
  console.log('req.url depois:', req.url);
  next();
});

app.get('/modificado', (req, res) => {
  res.send(`URL original: ${req.originalUrl}`);
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

/*
Se o user acessar -> http://localhost:3000/api/teste

Saída no console:
# req.url antes: /teste
# req.originalUrl: /api/teste
# req.url depois: /modificado

Resposta da API -> URL original: /api/teste

```

> Mesmo que `req.url` tenha sido alterado, `req.originalUrl` permaneceu intacto.


#### Resumo

#### Quando usar `req.originalUrl`?

1. **Registrar logs completos** das requisições feitas ao servidor.
2. **Evitar perda da URL original** em middlewares que modificam `req.url`.
3. **Criar redirecionamentos dinâmicos** baseados na URL original.
4. **Autenticação e autorização**: para verificar se o usuário tem acesso a uma URL específica.

Se precisar sempre da URL exata que o cliente enviou, **use `req.originalUrl` em vez de `req.url`**

---
---

### **`req.path`**

A propriedade **`req.path`** do objeto `request` no Express.js retorna **apenas o caminho da URL**, excluindo o domínio e a **query string** (parâmetros de consulta).

#### **Diferença entre `req.originalUrl`, `req.url` e `req.path`**

|Propriedade|O que retorna?|Inclui query string?|Pode ser alterado por middlewares?|
|---|---|---|---|
|`req.originalUrl`|Caminho completo da requisição, sem o domínio|✅ Sim|❌ Não|
|`req.url`|Caminho da requisição, podendo ser modificado por middlewares|✅ Sim|✅ Sim|
|`req.path`|Somente o **caminho**, sem a query string|❌ Não|❌ Não|

#### Estrutura básica

```js
// main.js

const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('req.path:', req.path);
  next();
});

app.get('/produto/:id', (req, res) => {
  res.send(`O caminho acessado foi: ${req.path}`);
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

/*
Se um cliente acessar -> `http://localhost:3000/produto/123?cor=azul&tamanho=M`

A saída no console será -> `req.path: /produto/123`

E a resposta da API será -> `O caminho acessado foi: /produto/123`

```


#### Comparando `req.originalUrl`, `req.url` e `req.path`

```js
// main.js

const express = require('express');
const app = express();

app.use('/api', (req, res, next) => {
  console.log('req.originalUrl:', req.originalUrl);
  console.log('req.url:', req.url);
  console.log('req.path:', req.path);
  next();
});

app.get('/api/teste', (req, res) => {
  res.send('Verifique o console!');
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));

/*
Se o usuário acessar -> `http://localhost:3000/api/teste?nome=joao`

Saída no console:
# req.originalUrl: /api/teste?nome=joao
# req.url: /teste?nome=joao
# req.path: /teste

```


#### Resumo

- **`req.originalUrl`** contém **toda a URL original**, incluindo a query string.
- **`req.url`** pode ser modificado por middlewares e inclui a query string.
- **`req.path`** contém **somente o caminho da URL**, sem query string.


#### **Quando usar `req.path`?**

1. **Verificação de rotas** sem se preocupar com query strings.
2. **Criação de middlewares dinâmicos**, baseados apenas no caminho.
3. **Filtragem de URLs** para logs ou regras de autenticação.

Se você **não precisa da query string**, `req.path` é mais limpo e direto do que `req.originalUrl`.

---
---


### **`req.cookie`**

No Express.js, a propriedade `req.cookies` é usada para acessar cookies enviados pelo cliente (navegador) na requisição HTTP. Porém, essa funcionalidade não está disponível por padrão no Express; é necessário usar o middleware `cookie-parser`.

#### **Entendendo `req.cookies`**

A propriedade `req.cookies` contém um objeto que representa os cookies enviados pelo cliente. Cada chave do objeto é o nome de um cookie e o valor é o conteúdo desse cookie.

#### **Passo a passo para usar `req.cookies` no Express**

#### **Instalar o middleware `cookie-parser`**

O Express não processa cookies automaticamente. Para isso, instalamos o pacote **`cookie-parser`**

```bash

npm install cookie-parser

```


#### **Importar e configurar `cookie-parser` no Express**

No arquivo principal do seu servidor (por exemplo, **`server.js`** ou **`app.js`**), você deve importar e configurar o **`cookie-parser`**:

```js
// main.js

const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

// Configurando o cookie-parser
app.use(cookieParser());

```


#### **Acessando cookies com `req.cookies`**

Depois de configurar o middleware, podemos acessar cookies enviados pelo cliente por meio da propriedade **`req.cookies`**.

```js
// main.js

app.get('/ver-cookies', (req, res) => {
    console.log(req.cookies); // Exibe os cookies recebidos
    res.send(req.cookies);
});

/*
Se um navegador enviar um cookie 
`user=JohnDoe`, o objeto `req.cookies` conterá:

{
  "user": "JohnDoe"
}

```


#### ### **Configurando Cookies no Cliente (via Servidor)**

Podemos definir cookies no cliente usando **`res.cookie()`**.

```js
// main.js

app.get('/set-cookie', (req, res) => {
    res.cookie('user', 'JohnDoe', { maxAge: 900000, httpOnly: true });
    res.send('Cookie foi definido!');
});

```


#### **Explicação:**

- **`'user'`:** nome do cookie.
- **`'JohnDoe'`:** valor do cookie.
- `{ maxAge: 900000 }`: tempo de expiração (em milissegundos).
- `{ httpOnly: true }`: impede que o cookie seja acessado via JavaScript no cliente.


#### **Cookies Assinados (`req.signedCookies`)**

Se quiser que os cookies sejam assinados para maior segurança, use:

```js
// main.js

app.use(cookieParser('minhaChaveSecreta'));

app.get('/set-signed-cookie', (req, res) => {
    res.cookie('user', 'JohnDoe', { signed: true });
    res.send('Cookie assinado foi definido!');
});

app.get('/ver-signed-cookies', (req, res) => {
    console.log(req.signedCookies);
    res.send(req.signedCookies);
});

```


#### **Diferença entre `req.cookies` e `req.signedCookies`**:

- `req.cookies`: contém todos os cookies normais.
- `req.signedCookies`: contém apenas cookies assinados.


#### **Resumindo**

1. **`req.cookies`** permite acessar cookies enviados pelo cliente.
2. Para usá-lo, é necessário configurar **`cookie-parser`.**
3. Podemos definir cookies com **`res.cookie()`.**
4. Cookies podem ser normais ou assinados (**`req.signedCookies`**).


---
---

### **`req.ip`**

A propriedade `req.ip` no Express.js retorna o endereço IP do cliente que fez a requisição para o servidor.

#### **Como funciona?**

Sempre que um cliente (navegador, Postman, outro servidor, etc.) faz uma requisição para o servidor Express, o Express pode capturar o endereço IP dessa requisição. Isso é útil para:

- **Monitoramento e logs**
- **Segurança** (exemplo: bloquear IPs suspeitos)
- **Personalização de conteúdo** (exemplo: restringir acesso por localização)

#### Estrutura básica

```js
// main.js

const express = require('express');
const app = express();

app.get('/meu-ip', (req, res) => {
    res.send(`Seu IP é: ${req.ip}`);
});

app.listen(3000, () => {
    console.log('Servidor rodando na porta 3000');
});

/*
Se você acessar `http://localhost:3000/meu-ip` no navegador, verá algo como:
Seu IP é: ::1

```

> Se estiver rodando localmente, pode aparecer `::1` (IPv6) ou `127.0.0.1` (IPv4), dependendo da configuração.



#### **Trabalhando com Proxies (`req.ips`)**

Se o Express estiver atrás de um proxy reverso (como Nginx ou Cloudflare), o IP real do cliente pode estar em cabeçalhos como `X-Forwarded-For`. Para que `req.ip` funcione corretamente nesses casos, você deve ativar o `trust proxy` no Express:

```js
//main.js

app.set('trust proxy', true);

```

#### Agora, você pode acessar a lista de IPs na propriedade `req.ips`:

```js
// main.js

app.get('/meu-ip', (req, res) => {
    res.send(`Seu IP real: ${req.ip} | Todos os IPs: ${req.ips}`);
});

```

#### **Explicação:**

- **`req.ip`:** Retorna o primeiro IP encontrado, geralmente o do cliente real.
- **`req.ips`:** Retorna um array de IPs, útil quando há múltiplos proxies.


#### ## **Exemplo com Bloqueio de IPs**

Podemos bloquear acessos de IPs específicos:

```js
// main.js

const bloqueados = ['192.168.1.100', '203.0.113.45'];

app.use((req, res, next) => {
    if (bloqueados.includes(req.ip)) {
        return res.status(403).send('Acesso negado');
    }
    next();
});

```


#### Resumo

- **`req.ip`** retorna o IP do cliente que fez a requisição.
- Pode ser **`127.0.0.1`** ou **`::1`** se estiver rodando localmente.
- Se houver proxies, ative **`app.set('trust proxy', true)`**.
- `req.ips` retorna todos os IPs quando há múltiplos proxies.
- Pode ser usado para segurança e logs.

