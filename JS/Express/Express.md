# Express JS
Node.js com Express é uma combinação poderosa para criar servidores web e APIs (CRUD). Express.js é um Framework minimalista para Node.js que facilita o gerenciamento de rotas, middlewares e requisições HTTP.

#### **Particularidades**

1.  Express **não serve arquivos de *diretórios arbitrários*** a menos que você os configure explicitamente **(static)**
	- ***Diretório arbitrário***, significa um diretório que você pode escolher livremente, sem estar preso a um caminho fixo ou pré-definido.

2. Os Objects **`req`** e o **`res.locals`** podem ser usados para armazenar dados temporários dentro de uma requisição no Express, mas existem diferenças importantes entre eles.
	- **Precisa passar dados entre middlewares e a rota final?** Use `req`.
	- **Precisa passar dados para um template renderizado?** Use `res.locals`.
	- **Quer armazenar dados específicos da resposta?** `res.locals` é a melhor escolha.
	
3. **Express.js** com **EJS** é uma combinação poderosa para criar aplicativos web dinâmicos e renderizar HTML de maneira eficiente no servidor. Saiba mais em [[EJS]]

| **Característica**         | **req**                                                         | **res.locals**                                                 |
| -------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------- |
| **Escopo**                 | Passa por todos os middlewares e a rota final.                  | Principalmente para templates e disponível apenas na resposta. |
| **Persistência**           | Dura toda a requisição.                                         | Dura toda a requisição, mas é usado na resposta.               |
| **Uso comum**              | Dados processados ao longo da requisição (exemplo: `req.user`). | Variáveis para renderização de templates no `res.render()`.    |
| **Acessível no template?** | Não diretamente.                                                | Sim, disponível em views do template engine (`res.render`).    |

### **Referências para Objects** 
#### **[[Res (Express)|res]], [[Req (Express)|req]], [[Req.session (seções)|session]], [[Req.flash (session)| flash]]**

---

#### Significado de CRUD 
(Create, Read, Update, Delete)

Create -> Post
Read -> Get
Update -> Pull
Delete -> Delete

## **Conceitos básicos**

- #### **Instalação**

```bash
npm init -y
npm install express # dependência do projeto
```


- #### **Criação servidor básico**
 
```js
server.js

const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello, Express!');
});

app.listen(3000, () => {
    console.log('Servidor rodando na porta 3000');
});
```

```bash
node server.js # comando para executador servidor

# Pronto, agora ao abrir um navegador e acessar o link "http://localhost:3000", verá a mensagem "Hello, express!". Está a a resposta configurada 
```

- **`app.get('/', callback)`:** Define uma rota para requisições `GET` na raiz (`/`).
- **`app.listen(port, callback)`:** Inicia o servidor na porta especificada.


---
---

## **Urls, Rotes e Params** 

- #### **URL (Uniform Resource Locator)**
É o endereço completo usado para acessar um recurso na web.
 https://www.exemplo.com/produtos/camiseta?id=123
 
 **Partes de uma URL:**
   - **Protocolo**: `https://` → Indica o tipo de conexão (HTTP ou HTTPS).
   - **Domínio**: `www.exemplo.com` → Nome do site.
   - **Caminho (Path)**: `/produtos/camiseta` → Indica o recurso específico dentro do site.
   - **Parâmetros de consulta (Query Parameters)**: `?id=123` → Enviado na URL para fornecer informações adicionais.

#### Representação d URLs 
```bash
echo "https://www.exemplo.com/produtos/camiseta?id=123";
```


---
---


- #### **Rotes**
São os caminhos definidos dentro de um servidor ou aplicação para direcionar as solicitações do usuário.

- No **backend**, uma rota pode ser `/produtos` e retornar uma lista de produtos.
- No **frontend** de uma **SPA (Single Page Application)**, uma rota pode definir qual componente será exibido.**

#### Representação de Rotes
```bash
# Rotas
echo " https://www.exemplo.com/produtos/camiseta?id=123"
```

#### Exemplo:
**Rotes** em Node.js com Express:

```js
app.get('/produtos', (req, res) => {
    res.send('Lista de produtos');
});
```


---
---


#### **Params** 
**Parâmetros de rota** (Path Parameters): Fazem parte da URL e são usados para identificar recursos.

**Exemplo:** `/produtos/:id`
* Se acessarmos `/produtos/123`, o ID `123` será extraído.
 
#### Exemplo:
 **Params** em NodeJS com  Express:

```js
app.get('/produtos/:id', (req, res) => {
    res.send(`Produto ID: ${req.params.id}`);
});
```

