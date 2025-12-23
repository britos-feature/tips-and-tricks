# Express (req, res)

No contexto do **Node.js** e do Framework **Express.js**, os objetos **`req` (request)** e **`res` (response)** são fundamentais para lidar com requisições HTTP e enviar respostas. Eles são passados automaticamente como parâmetros para as funções de middleware e rotas.

## **res**

O objeto **`res`** é uma instância de **`http.ServerResponse`**, herdado do módulo **`http`** do Node.js, e o Express adiciona métodos próprios para facilitar o envio de respostas. Isso significa que ele herda métodos nativos da API HTTP do Node.js e adiciona novas funcionalidades para facilitar o desenvolvimento de aplicações web.

- **Origem:** `res` vem do Node.js (**`http.ServerResponse`**).
- **Expansão:** O Express adiciona métodos próprios para facilitar manipulação de respostas HTTP.
- **Encerramento:** Uma resposta pode ser encerrada implicitamente (**`res.send()`**) ou explicitamente (**`res.end()`**).

#### **Métodos de Envio de Resposta**

#### `res.send([body])`

Envia uma resposta HTTP com um corpo (body). O tipo do `body` pode ser:
**Curiosidades Internas**

- Se **`body`** for um **objeto**, o Express chama `res.json(body)`.
- Se **`body`** for **um Buffer**, ele é enviado como binário.
- Se **`body`** for **uma string**, o Express define **`Content-Type` como `text/html`** ou **`text/plain`**, dependendo do conteúdo.
- **Booleano ou null** → Converte para [JSON]().

```js
//main.js

res.send('<h1>Olá, Express!</h1>'); // Responde com HTML
res.send({ mensagem: 'Sucesso!' }); // Responde com JSON
res.send([1, 2, 3]); // Responde com JSON

```

#### `res.json([body])`

Similar a `res.send()`, mas garante que a resposta seja JSON e configura **`Content-Type: application/json`**.

- Sempre retorna um JSON válido.
- Adiciona **`Content-Type: application/json`.**
- Faz _stringify_ do objeto automaticamente.

```js
// main.js

app.get('/erro', (req, res) => {
  res.json(null); // Responde com "null" e Content-Type: application/json
});


res.jsonp({ mensagem: 'Isso é JSONP' });

```


#### `res.jsonp([body])`

Semelhante a **`res.json()`**, mas suporta _JSONP_ (JSON com _padding_), útil para requisições _cross-origin_ via **`<script>`.**

```js
// main.js

res.jsonp({ mensagem: 'Isso é JSONP' });

```

> Se a query string contiver um parâmetro `callback`, o Express encapsula a resposta em uma função JavaScript.


#### `res.sendFile(path, [options], [callback])`

Envia um arquivo estático ao cliente.

**Opções úteis**

- **`root`:** Define um diretório base para evitar caminhos absolutos inseguros.
- **`maxAge`:** Define tempo de cache em milissegundos.
- **`lastModified`:** Habilita/desabilita o cabeçalho `Last-Modified`.

```js
// main.js

res.sendFile(__dirname + '/public/index.html');


app.get('/baixar', (req, res) => {
  res.sendFile('relatorio.pdf', { root: __dirname });
});

```


#### `res.download(path, [filename], [callback])`

Força o download de um arquivo.

```js
//main.js

res.download('relatorio.pdf');

```

---
---

### **Definição do Status HTTP**

O Express permite definir códigos HTTP de resposta com `res.status()`, mas há alguns detalhes importantes.

#### `res.status(code)`

Define o código de status da resposta.

```js
// main.js

res.status(404).send('Página não encontrada');
res.status(500).json({ erro: 'Erro interno' });

```


#### `res.sendStatus(code)`

Atalho para definir o código de status e enviar sua descrição como resposta.

```js
// main.js

res.sendStatus(404); // Equivalente a res.status(404).send('Not Found')

```


#### Diferença entre `res.status()` e `res.sendStatus()`

```js
// main.js 

res.status(404).send('Página não encontrada'); // Envia "Página não encontrada"
res.sendStatus(404); // Envia "Not Found"

```

---
---

### **Manipulação de Cabeçalhos**

Os cabeçalhos HTTP definem informações sobre a resposta e o comportamento do navegador.

#### `res.set(field, [value])`

Define um ou mais cabeçalhos de resposta.

```js
// main.js

res.set('Content-Type', 'text/plain');
res.set({
  'X-Custom-Header': 'Valor personalizado',
  'Cache-Control': 'no-store'

// internamente
res.setHeader(field, value);

});

```


#### `res.get(field)`

Obtém o valor de um cabeçalho específico.

```js
// main.js

const tipo = res.get('Content-Type');
console.log(tipo);

```


##### `res.append(field, value)`

Adiciona um valor a um cabeçalho (em vez de sobrescrevê-lo).

```js
// main.js

res.append('Set-Cookie', 'sessionId=abc123');
res.append('Set-Cookie', 'userId=456xyz');

```


#### `res.removeHeader(field)`

Remove um cabeçalho da resposta.

```js
// main.js

res.removeHeader('X-Powered-By');

```

---
---

### **Redirecionamentos**

#### `res.redirect([status,] path)`

Redireciona para outra URL.

```js
// main.js

res.redirect('/outra-pagina');
res.redirect(301, 'https://google.com'); // Redirecionamento permanente

// Internamente
res.setHeader('Location', url);
res.statusCode = status;
res.end();

```

---
---

### **Cookies**

Os cookies permitem armazenar dados no navegador do usuário.

#### `res.cookie(name, value, [options])`

Define um cookie na resposta.

**Opções úteis**

- `maxAge`: Tempo de vida em milissegundos.
- `secure`: Envia apenas via HTTPS.
- `httpOnly`: Protege contra acesso via JavaScript.
- `sameSite`: Protege contra ataques CSRF.

```js
// main.js

res.cookie('token', 'abc123', { httpOnly: true, secure: true });

res.cookie('token', 'abc123', { httpOnly: true, secure: true, maxAge: 3600000 });


```


#### `res.clearCookie(name, [options])`

Remove um cookie.

```js
// main.js

res.clearCookie('token');

```

---
---


### **Encerramento da Resposta**

#### `res.end([data])`

Finaliza a resposta manualmente.

```js
// main.js

res.end('Fim da resposta');

```


#### Exemplo completo

```js
// main.js

const express = require('express');
const app = express();

app.get('/json', (req, res) => {
  res.status(200)
    .set('X-Custom-Header', 'MeuApp')
    .cookie('sessionId', 'abc123', { httpOnly: true })
    .json({ mensagem: 'Olá, Express!' });
});

app.get('/download', (req, res) => {
  res.download('arquivo.pdf');
});

app.get('/redirect', (req, res) => {
  res.redirect(301, 'https://meusite.com');
});

app.get('/headers', (req, res) => {
  res.set('Cache-Control', 'no-store');
  res.send('Cabeçalhos definidos');
});

app.get('/cookie', (req, res) => {
  res.cookie('user', 'joao', { maxAge: 900000, httpOnly: true });
  res.send('Cookie setado!');
});

app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});

```
