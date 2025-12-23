O **nodemon** é uma ferramenta muito útil no desenvolvimento com **Node.js**, pois ele **reinicia automaticamente o servidor** sempre que você faz alguma **mudança no código**.

O **`nodemon`** é essencialmente um **monitor de arquivos**.  
Ele observa mudanças em arquivos do seu projeto usando o sistema de **watch** do Node.js (ou da biblioteca `chokidar`, internamente).

Quando detecta uma modificação (salvamento, criação, exclusão), ele:

1. **encerra o processo atual** do Node;
2. **reinicia** o programa automaticamente.

Assim, o servidor Node fica sempre ativo, refletindo suas mudanças **em tempo real** sem você precisar parar e rodar de novo.

Sem o **nodemon**, toda vez que você altera o código de um arquivo (por exemplo, `app.js`), precisa parar manualmente o servidor (`Ctrl + C`) e iniciar de novo com **`node app.js`**.  

### Como instalar o nodemon
Você pode instalar globalmente ou apenas no projeto.

- **Instalação Global

```sh

npm init -y
npm i -g nodemon

```

> Assim, você pode usar o comando `nodemon` em qualquer projeto.
<br>
- **Instalação local (no project)**

```sh

npm init -y
npm i --save-dev nodemon

```

> O **`--save-dev`** indica que ele é usado apenas em desenvolvimento, não em produção.
<br>
### Como usar o nodemon

**Exemplo básico**

```sh

# Sem o nodemon
node app.js

```

```sh

# Com o nodemon
nodemon app.js

```

> Agora, qualquer alteração em **`app.js`** ou nos arquivos do projeto fará o nodemon reiniciar automaticamente o servidor.
<br>
### Configuração com **`package.json`**
Você pode facilitar ainda mais criando um **script**

```json
// package.json

{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js"
  }
}

```

```sh

# Executando - nodemon no script
npm run dev

```
<br>
### Configuração **`nodemon.json`** (opcional)

Você pode personalizar totalmente o comportamento do nodemon criando um arquivo **nodemon.json** na raiz do projeto.



- **Definição de extensão a monitor**

```json

{
  "watch": ["src", "routes"],
  "ext": "js,json,html",
  "ignore": ["node_modules", "logs/*"],
  "delay": "1000",
  "exec": "node --inspect app.js"
}

```

> **Opção e Descrição:**
> 	- **`watch`** -> Pastas/arquivos que o nodemon deve observar
> 	- **`ext`** -> Extensões de arquivos que disparam reinício
> 	- **`ignore`** -> Itens que o nodemon deve ignorar
> 	- **`delay`** -> Tempo (ms) para esperar antes de reiniciar
> 	- **`exec`** -> Comando personalizado para rodar o app
<br>
### Resumo:

| Função                 | Descrição                                            |
| ---------------------- | ---------------------------------------------------- |
| 🔄 Reinicia automático | Reinicia o servidor ao detectar mudanças             |
| ⚙️ Fácil integração    | Pode ser configurado pelo `package.json`             |
| 🧰 Desenvolvimento     | Ideal para ambiente de desenvolvimento, não produção |
	