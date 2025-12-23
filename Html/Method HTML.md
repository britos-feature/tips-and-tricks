Nesse artigo iremos entender **detalhadamente** o que são os **métodos HTML**, especialmente no contexto de **formulários (`<form>`)**, pois é nesse elemento **HTML** que o atributo **`method`** é usado e tem real funcionamento.

### Atributo **_`method`_** no HTML

O atributo `method` é usado dentro da tag `<form>` e **define como os dados de um formulário serão enviados para o servidor** (ou para outro recurso).

**Estrutura básica:**

```html

<form action="/destino" method="get">
  <input type="text" name="usuario" />
  <input type="password" name="senha" />
  <button type="submit">Enviar</button>
</form>

```
<br>
- **action** → indica para onde os dados serão enviados (**URL** ou **rota**).
- **method** → indica como os dados serão enviados (**GET**, **POST**, etc...).

---

## Métodos principais e seus funcionamentos

O HTML padrão reconhece dois métodos principais:

<h2><code>GET</code> (padrão)</h2>
Usado, quando você não especifica um `method`, o HTML usa **GET** por padrão.
#### Funcionamento:
- Os dados do formulário são enviados **na URL** como <i><code>parâmetros</code></i>.

**Exemplo "URL como parâmetros":**  `/destino?usuario=alex&senha=123`
  
- Os valores são **visíveis** na barra de endereços.
- Os dados podem ser **cacheados** e **armazenados** no histórico.
- O tamanho é **limitado** (geralmente até ~2000 caracteres).

### Quando usar:

- Quando os dados **não são sensíveis** (ex: buscas, filtros).
- Quando você quer permitir que o link seja **compartilhado** ou **marcado**.
    

### Quando evitar:

- Quando os dados são **privados** (ex: senhas, dados pessoais).
- Quando o envio é **grande** (arquivos, textos longos, etc).

### Exemplo prático:

```html
<form action="/buscar" method="get">
  <input type="text" name="q" placeholder="Buscar..." />
  <button>Pesquisar</button>
</form>
```

**Resultado:**   URL final → **`/buscar?q=chatgpt`**

---

<h2><code>POST</code> (padrão)</h2>

Usado, quando envia os dados **no corpo (body)** da requisição HTTP, **não na URL**.

#### Funcionamento:
- Os dados **não aparecem** na barra de endereços.
- Não há limite prático de tamanho (ideal para uploads ou textos longos).
- O navegador **não armazena** o envio no histórico.
- Usado para **criar, alterar ou enviar informações confidenciais**.

### Quando usar:

- Para **formulários de login**, **cadastro**, **uploads**, **envios de dados sigilosos**.

#### Exemplo prático:

```html
<form action="/login" method="post">
  <input type="text" name="usuario" />
  <input type="password" name="senha" />
  <button type="submit">Entrar</button>
</form>
```

> Os dados são enviados **internamente** no corpo da requisição HTTP (não visíveis na URL).

---

## Métodos menos comuns (HTML + APIs modernas)

Esses métodos **não são nativos do HTML**, mas aparecem em **requisições HTTP** feitas por JavaScript (ex: `fetch()` ou `axios`). O HTML puro **não os reconhece diretamente no `<form>`**, mas são importantes:

| Método   | Descrição                                 | Uso comum |
| -------- | ----------------------------------------- | --------- |
| `PUT`    | Atualiza um recurso existente no servidor | APIs REST |
| `DELETE` | Remove um recurso no servidor             | APIs REST |
| `PATCH`  | Atualiza parcialmente um recurso          | APIs REST |

#### Esses métodos são usados via JavaScript:

```js
fetch('/api/usuario/1', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ nome: 'Alex' })
});
```


---

### Diferença técnica entre GET e POST (resumo):

|Característica|GET|POST|
|---|---|---|
|Local dos dados|URL|Corpo da requisição|
|Visível na barra de endereço|✅ Sim|❌ Não|
|Tamanho limitado|✅ Sim|❌ Não|
|Cacheável|✅ Sim|❌ Não|
|Ideal para|Consultas, buscas|Login, envio de dados|
|Segurança|❌ Menos segura|✅ Mais segura (HTTPS recomendado)|

---

### Extras úteis em formulários

Além do **`method`**, existem outros atributos que interagem com ele:

|Atributo|Função|
|---|---|
|`action`|URL de destino (para onde enviar os dados)|
|`enctype`|Tipo de codificação dos dados (ex: `multipart/form-data` para upload)|
|`target`|Onde abrir o resultado (`_blank`, `_self`, etc)|
|`autocomplete`|Controla o preenchimento automático do navegador|

Exemplo com upload:

```html
<form action="/upload" method="post" enctype="multipart/form-data">
  <input type="file" name="arquivo" />
  <button type="submit">Enviar arquivo</button>
</form>
```

---

## Conclusão

O atributo `method` define **como** os dados de um formulário HTML serão enviados:

- **GET** → simples, transparente, limitado e público.
- **POST** → seguro, completo, usado em praticamente todos os formulários reais.
- Outros métodos (PUT, DELETE, PATCH) → usados apenas via **JavaScript** ou frameworks.

---


## Visualização **na prática** como o navegador realmente envia os dados em uma requisição **GET** e **POST**.  

Entendimento de como acontece **“por baixo do capô”** quando um formulário é enviado.


### Método `GET`

#### Código HTML:

```html
<form action="/login" method="get">
  <input type="text" name="usuario" value="alex" />
  <input type="password" name="senha" value="12345" />
  <button type="submit">Entrar</button>
</form>
```
<br>
#### Quando o usuário clica em “Entrar”…

O navegador monta a requisição **HTTP GET** assim:

```
GET /login?usuario=alex&senha=12345 HTTP/1.1
Host: exemplo.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64)
Accept: text/html,application/xhtml+xml,application/xml
Accept-Language: pt-BR,pt;q=0.9
Connection: keep-alive
```

**Observações:**

- Os dados (`usuario` e `senha`) estão **na URL** (`?usuario=alex&senha=12345`).
- Essa URL pode ser **armazenada no histórico**, **cacheada** e até **compartilhada**.
- Por isso **não deve ser usada com informações sensíveis**.
    

---

### Método `POST`

#### Código HTML:

```html
<form action="/login" method="post">
  <input type="text" name="usuario" value="alex" />
  <input type="password" name="senha" value="12345" />
  <button type="submit">Entrar</button>
</form>
```

#### Quando o usuário envia o formulário…

O navegador monta a requisição **HTTP POST** assim:

```
POST /login HTTP/1.1
Host: exemplo.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64)
Accept: text/html,application/xhtml+xml,application/xml
Accept-Language: pt-BR,pt;q=0.9
Content-Type: application/x-www-form-urlencoded
Content-Length: 26
Connection: keep-alive

usuario=alex&senha=12345
```

📍 **Observações:**

- Agora os dados (`usuario` e `senha`) estão **no corpo da requisição**, e não na URL.
- A URL continua limpa: `/login`
- O navegador envia os dados em formato **URL-encoded**, semelhante a pares `chave=valor`.
- Com HTTPS, tudo no corpo da requisição fica **criptografado**.
    

---

### Comparativo visual

|Parte|GET|POST|
|---|---|---|
|Linha inicial|`GET /login?usuario=alex&senha=12345`|`POST /login`|
|Cabeçalhos|Sim|Sim|
|Corpo da requisição|❌ Não tem|✅ `usuario=alex&senha=12345`|
|Visibilidade|Alta (na URL)|Baixa (no corpo)|
|Segurança|Fraca|Forte (especialmente com HTTPS)|

---

### E como isso aparece no servidor?

Em um backend (por exemplo, Node.js com Express), ficaria assim:

```js
app.get('/login', (req, res) => {
  console.log(req.query); // Dados do GET
  // Ex: { usuario: 'alex', senha: '12345' }
});

app.post('/login', (req, res) => {
  console.log(req.body); // Dados do POST
  // Ex: { usuario: 'alex', senha: '12345' }
});
```

 **`req.query`** → dados vindos pela URL (GET)  
 **`req.body`** → dados vindos no corpo da requisição (POST)

---

### Extra: Envio de arquivos (`multipart/form-data`)

Se o formulário tiver `<input type="file">`, o método **deve ser POST**, e você precisa definir o `enctype`:

```html
<form action="/upload" method="post" enctype="multipart/form-data">
  <input type="file" name="arquivo" />
  <button type="submit">Enviar</button>
</form>
```

Nesse caso, o corpo da requisição muda completamente — ele envia **blocos binários** com **fronteiras** (boundary markers) que separam cada campo e arquivo.

---

### Resumo Final

| Método   | Local dos dados        | Visível na URL | Segurança          | Ideal para                 |
| -------- | ---------------------- | -------------- | ------------------ | -------------------------- |
| **GET**  | Na URL                 | ✅ Sim          | ❌ Baixa            | Pesquisas, filtros         |
| **POST** | No corpo da requisição | ❌ Não          | ✅ Alta (com HTTPS) | Logins, cadastros, uploads |

---

### Referências a METHODS, encontrados em APP/ APIS / PROJECT

#### Names:

- **index** → Lista todos os usuários → **GET**
- **store/create** → Cria novos usuários → **POST**
- **delete** → Apaga um usuário → **DELETE**
- **show** → Mostra um usuário → **GET**
- **update** → Atualiza um usuário → **PATCH** ou **PUT**
	- **PATCH** → Atualiza apenas um valor.
	- **PUT** → Atualiza o object inteiro (all value)


---

