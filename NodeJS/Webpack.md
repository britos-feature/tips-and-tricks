O **Webpack** é um **empacotador de módulos** (_module bundler_) muito usado em projetos JavaScript modernos. Ele pega todos os arquivos e recursos do seu projeto — como **JS, CSS, imagens, fontes** e até **HTML** — e os combina (ou “empacota”) em um ou mais arquivos otimizados para uso no **navegador**.

A partir disso, ele **gera um único arquivo final (bundle)** — geralmente chamado **`bundle.js`** — que o navegador pode carregar rapidamente.

### **Conceitos fundamentais básicos do Webpack**

- **Entry (entrada)** 
	- O ponto de entrada principal do seu projeto. É o arquivo (ou arquivos) onde o Webpack começa a construir a árvore de dependências.

```js

entry: './src/index.js'

```

- **Output (saída)**  
	- Define onde e com que nome os arquivos empacotados serão gerados.

```js

output: {
  filename: 'bundle.js',
  path: path.resolve(__dirname, 'dist'),
}

```

- **Loaders**  
	- O Webpack por padrão só entende JS/JSON. Os **loaders** permitem transformar outros tipos de arquivos (como CSS, Sass, imagens, ou TypeScript) em módulos válidos de JS.  
		- Exemplo: usar `css-loader` e `style-loader` para importar CSS nos arquivos JS.

```js

module: {
  rules: [
    {
      test: /\.css$/i,
      use: ['style-loader', 'css-loader'],
    },
  ],
}

```

- **Plugins**
	- Plugins expandem as capacidades do Webpack. Eles são usados para tarefas como minificação, injeção de arquivos no HTML, limpeza da pasta de build, entre outros.
		- Exemplo: `HtmlWebpackPlugin` injeta seu bundle JS no HTML automaticamente.

```js

const HtmlWebpackPlugin = require('html-webpack-plugin');

plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html',
  }),
]

```

- **Mode** 
	- Define o modo de ***build***
		- **`'development'`**: build rápido, sem minificação, com sourcemaps.
		- **`'production'`**: minificado e otimizado.

```js

mode: 'development' // ou 'production'

```

- **DevServer** (via `webpack-dev-server`)  
	- Um servidor local que recarrega automaticamente a aplicação quando os arquivos mudam. Ideal para desenvolvimento.

```js

devServer: {
  static: './dist',
  hot: true,
  open: true,
}

```


### **Exemplo básico (webpack)**

```js

// configuração básica

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true, // limpa a pasta dist antes do build
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ],
  mode: 'development',
  devtool: 'source-map',
};

```


###  **Por que usar Webpack?**

- Empacotamento de arquivos em um só.
- Suporte a módulos modernos (ES6, CommonJS).
- Processamento de CSS, imagens, e mais.
- Otimização para produção.
- Extensível com loaders e plugins.


### **Configurando Webpack**

#### Inicializando project

```shell

npm init -y

```

---
---

#### Instalando pacotes para webpack

Instalar Webpack e o Webpack CLI como dependência de desenvolvimento

```shell
npm install --save-dev webpack webpack-cli
```

---
---

### Estrutura básica para o project

```pgsql
meu-projeto/

├── src/
│ └── index.js
├── dist/
│ └── index.html
├── package.json
```

---
---

### Arquivo de configuração do Webpack (opcional, mas recomendado)

```js
const path = require("path");
module.exports = {
	entry: "./src/index.js", // ponto de entrada
	output: {
		filename: "bundle.js", // saída
		path: path.resolve(__dirname, "dist"), // pasta de saída
	},
	mode: "development", // ou 'production'
};
```

---
---

### Package.json **script**

```json
{
	"script": {
		"build": "webpack"
	}
}
```

---
---
 
### Arquivo src/index.js (entrada)

```js
console.log("Hello World!");
```

---
---

### Arquivo dist/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>Webpack App</title>
</head>
<body>
	<script src="bundle.js"></script>
</body>
</html>
```


### Compilar o projeto com Webpack

```shell
npm run build

# Isso vai gerar dist/bundle.js a partir de src/index.js.
```


---
---

## **Configurações básica Webpack com BabelJS**

### **Pacotes Webpack (Empacotador de módulos)**

#### Instalação principal:

```shell
npm install --save-dev webpack webpack-cli webpack-dev-server
```

#### **Explicação:**
- **`webpack`** – O próprio empacotador.
- **`webpack-cli`** – Interface de linha de comando do Webpack.
- **`webpack-dev-server`** – Servidor local de desenvolvimento com hot reload.


### **Pacotes Babel (Transpilador JS)**

#### Instalação principal:

```shell
npm install --save-dev @babel/core @babel/preset-env babel-loader
```

#### **Explicação:**
- **`@babel/core`** – Núcleo do Babel.
- **`@babel/preset-env`** – Preset que permite compilar código ES6+ para versões compatíveis com navegadores antigos.
- **`babel-loader`** – Permite integrar Babel com Webpack.


### **Outros pacotes úteis (Recomendado)**
#### Para gerar um arquivo HTML automaticamente:

```shell
npm install --save-dev html-webpack-plugin
```

#### Para limpar a pasta `dist` antes de cada build:

```shell
npm install --save-dev clean-webpack-plugin
```

#### Para trabalhar com arquivos CSS:

```shell
npm install --save-dev style-loader css-loader
```


### **Exemplo de arquivo `webpack.config.js`:**

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true, // limpa a pasta dist (a partir do Webpack 5)
  },
  mode: 'development', // ou 'production'
  devServer: {
    static: './dist',
    hot: true,
    port: 3000,
  },
  module: {
    rules: [
      {
        test: /\.m?js$/, // aplica em arquivos JS
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      }
    ],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'Projeto JS com Babel e Webpack',
      template: './src/index.html',
    }),
  ],
};
```


### **Exemplo de arquivo `.babelrc`:**

```json
{
  "presets": ["@babel/preset-env"]
}
```


### **Scripts no `package.json`:**

```json
"scripts": {
  "start": "webpack serve --open",
  "build": "webpack --mode production"
}
```


### **EXPLICAÇÃO ( regenerator-runtime, core-js )
Quando utilizamos **Babel** para transpilar o código moderno (ES6+) para versões mais antigas de JavaScript, surgem dois tipos de necessidades:

- **Sintaxe:** O Babel converte coisas como **arrow functions**, classes, etc.

- **APIs:** Babel não injeta APIs ausentes no ambiente de execução (ex.: `Promise`, `Array.from`, `async/await`). Para isso, são necessários **polyfills** (bibliotecas).


# **core-js**
É uma biblioteca que fornece **polyfills** para APIs modernas do JavaScript (ex.: `Promise`, `Array.includes`, `Object.entries`).

Funciona junto com Babel para incluir automaticamente apenas os **polyfills** necessários, dependendo do seu alvo (browsers ou Node.js).

#### **Instalação**

```shell
npm install core-js
```


#### Exemplo de configuração no `.babelrc`:

```json
{
  "presets": [
    ["@babel/preset-env", {
      "useBuiltIns": "usage",
      "corejs": "3.37"
    }]
  ]
}
```

***`useBuiltIns`*** opções:

- **`"usage"`** → Adiciona polyfills automaticamente com base no que você usa no código.
- **`"entry"`** → Você adiciona manualmente uma linha de importação no seu arquivo principal (`index.js`)

```js
import 'core-js/stable';
import 'regenerator-runtime/runtime';
```


# **regenerator-runtime**
É Necessário para dar suporte a funções **`async/await`** e geradores (`function*`).

 Quando **Babel** transpila **`async/await`**, ele converte internamente em geradores, que precisam do **runtime do regenerator**

#### **Instalação**

```shell
npm install regenerator-runtime
```

> Se você usa **`"useBuiltIns": "usage"`** no Babel, ele cuida disso automaticamente.
> Se usar **`"entry"`**, deve importar manualmente no seu arquivo de entrada (`index.js`):

```js
import 'regenerator-runtime/runtime';
```


#### **`.babelrc` ou `babel.config.json`** <span style="color: red">(necessário só para <b>babelJS</b> sem webpack)</span>

```json
{
  "presets": [
    ["@babel/preset-env", {
      "useBuiltIns": "usage",
      "corejs": "3.37"
    }]
  ]
}
```


#### ### **Arquivo de entrada (`index.js`):**
Utilizando  **`"entry"` (webpack.config.js)**

```js
import "regenerator-runtime/runtime";
import "core-js/stable"
```

## **Importante!**

- O uso de `core-js` **ainda é obrigatório** para garantir compatibilidade com navegadores mais antigos, dependendo da configuração de **`@babel/preset-env`**.

- O `regenerator-runtime` continua sendo utilizado para suportar **`async/await`** em ambientes que não suportam nativamente (browsers antigos, versões antigas de Node.js).


## **Simplesmente:**
Se você usa ES6+, async/await e quer rodar seu código em qualquer navegador (ou Node.js antigo), **você ainda precisa de `core-js` e `regenerator-runtime`** junto com Babel.