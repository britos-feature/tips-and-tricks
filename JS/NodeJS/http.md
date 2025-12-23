# Module HTTP - NodeJS

O módulo http no Node.js é um dos principais módulos embutidos e é usado para criar servidores HTTP e fazer requisições HTTP. 
Ele permite que você construa aplicações web, APIs e clientes HTTP de maneira eficiente.

### Importando o módulo http 
Como o http é um módulo nativo do Node.js, você pode importá-lo diretamente sem precisar instalá-lo via **npm**.

```js
const http = require('node:http');
```



## **Criando um servidor HTTP**

Você pode criar um servidor HTTP usando o método **http.createServer().** 
Esse método recebe um callback com dois parâmetros principais:
    **req (request)** → Representa a requisição do cliente.
    **res (response)** → Representa a resposta do servidor.

#### Exemplo básico

#### Criando o servidor

```js
const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' }); // Define o status HTTP e o tipo de conteúdo
    res.end('Olá, mundo!'); // Envia a resposta para o cliente
});

// O servidor escuta na porta 3000
server.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000/');
});
```

**Explicação**:
- **http.createServer()** cria um servidor que processa requisições HTTP.
- O callback recebe req (dados da requisição) e res (resposta do servidor).
- **res.writeHead(200, { 'Content-Type': 'text/plain' })** define o status da resposta (200 OK) e o tipo de conteúdo (text/plain).
- **res.end('Olá, mundo!')** envia a resposta e finaliza a conexão.
- **server.listen(3000, () => { ... })** inicia o servidor na porta 3000



#### Lidando com Rotas (Endpoints)

Podemos verificar a URL da requisição e responder de maneira diferente com base nela

```js
const server2 = http.createServer((req, res) => {
    if (req.url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>Bem-vindo à página inicial!</h1>');
    } else if (req.url === '/sobre') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>Sobre nós</h1>');
    } else {
        res.writeHead(404, { 'Content-Type': 'text/html' });
        res.end('<h1>Página não encontrada</h1>');
    }
});

server2.listen(3001, () => {
    console.log('Servidor rodando em http://localhost:3001/');
});
```

 **Como funciona:**
	 Se o usuário acessar http://localhost:3000/, verá "Bem-vindo à página inicial!".
	 Se acessar http://localhost:3000/sobre, verá "Sobre nós".
	 Se tentar acessar qualquer outra rota, receberá um erro 404 Página não encontrada.
	 

#### Lendo dados da requisição (Método HTTP e Cabeçalhos)

Podemos verificar o método HTTP (GET, POST, etc.) e os cabeçalhos da requisição.

```js
const server3 = http.createServer((req, res) => {
    console.log(`Método: ${req.method}`);
    console.log(`URL: ${req.url}`);
    console.log('Cabeçalhos:', req.headers);

    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Verifique o console para detalhes da requisição');
});

server3.listen(3002, () => {
    console.log('Servidor rodando em http://localhost:3002/');
});
```

> Quando você acessar http://localhost:3002/, o console do Node.js exibirá os detalhes da requisição


#### Manipulando Requisições POST (Lendo o Corpo da Requisição)

Se um cliente enviar dados via POST, podemos capturar o corpo da requisição assim. */

```js
const server4 = http.createServer((req, res) => {
  if (req.method === 'POST') {
    let body = '';

    // Capturando os dados aos poucos
    req.on('data', chunk => {
      body += chunk.toString();
    });

    // Quando todos os dados forem recebidos
    req.on('end', () => {
      res.writeHead(200, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ mensagem: 'Dados recebidos!', dados: body }));
    });
  } else {
    res.writeHead(405, { 'Content-Type': 'text/plain' });
    res.end('Método não permitido');
  }
});

server4.listen(3003, () => {
  console.log('Servidor rodando em http://localhost:3003/');
});
```

**Como funciona:**
	Quando um cliente faz uma requisição POST, os dados chegam em partes (chunks).
    O evento **req.on('data')** captura os pedaços de dados e os junta.
    O evento **req.on('end')** indica que todos os dados chegaram.
    O servidor responde com uma mensagem JSON confirmando o recebimento.

Você pode testar isso com curl ou Postman
curl -X POST https://localhost:3003/ -d "nome=João&idade=25"


#### Fazendo Requisições HTTP com http.request()

O módulo http também permite que você faça requisições HTTP como cliente.
Exemplo de requisição GET para um site externo. */

```js
const options = {
  hostname: 'jsonplaceholder.typicode.com',
  path: '/todos/1',
  method: 'GET',
};

const req = http.request(options, res => {
  let data = '';

  res.on('data', chunk => {
    data += chunk;
  });

  res.on('end', () => {
    console.log(JSON.parse(data));
  });
});

req.on('error', error => {
  console.error('Erro na requisição:', error);
});

req.end();
```



#### Melhorando o Servidor com JSON e API Simples

Podemos criar uma API que retorna JSON em vez de HTML. */

```js
const server5 = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({ mensagem: 'Olá, mundo!', sucesso: true }));
});

server5.listen(3004, () => {
  console.log('API rodando em http://localhost:3004/');
});
```


### Resumo

 - O módulo http do Node.js é muito poderoso e permite criar servidores web e clientes HTTP de forma eficiente. 
 - No entanto, em aplicações mais complexas, geralmente usamos frameworks como Express.js para facilitar o desenvolvimento. 
