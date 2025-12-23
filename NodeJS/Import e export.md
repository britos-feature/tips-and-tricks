No Node.js existem **dois sistemas de módulos principais** para utilizar **importação e exportação**.

## 1. **CommnonJS (CJS)** -  _padrão mais antigo_

 -  Usa **`require()`** para importar e **`module.exports`** ou **`exports`** para exportar.

**Resumindo**
No **CommonJS**, as formas principais de exportar são:

1. ***`module.exports = valor`** → exporta qualquer coisa (função, classe, objeto, etc.)
2. **`module.exports = { ... }`** → exporta múltiplos itens como objeto
3. **`exports.nome = valor`** → adiciona propriedades ao objeto exportado
4. Incremental: **`module.exports.algo = ...`**    
5. Exportações dinâmicas/condicionais

Ou seja, **tudo no CommonJS gira em torno do objeto `module.exports`**.  
`exports` é só uma variável de conveniência que aponta para o mesmo objeto (até você reatribuir).

**_OBS:_**
	_No NodeJS na exportação utilizando **CommonJS**, a variável **`exports`** por padrão aponta para **`module.exports`**.
	Então nesse contexto de exportação do NodeJS, a palavra **`this`** aponta tanto para **`exports`** quanto para **`module.exports`**_


`module.exports = { greeting: () => "Hello"; }`
`module.exports.greeting = () => "Hello";`
`exports.greeting = () => "Hello";`
**`this.greeting = () => "Hello";`**

<span style="color:blue"><big>./codes/commonJS(import-export)</big></span>, modelos e exemplos mais detalhados!.

**Sintaxe básica**

```js
// exportando

// arquivo math.js
function soma(a, b) {
  return a + b;
}

function sub(a, b) {
  return a - b;
}

// Exporta múltiplas funções como objeto
module.exports = { soma, sub };


// importando

// arquivo app.js
const math = require('./math');

console.log(math.soma(2, 3)); // 5
console.log(math.sub(5, 2));  // 3

```

**OR**<br>
```js

// exportando direto

// arquivo mult.js
module.exports = function(a, b) {
  return a * b;
};

// importando

const mult = require('./mult');
console.log(mult(2, 4)); // 8

```
<br>
## 2. **ES Module(MJS)** - _ESM, mais moderno_

-  Usa **`import`** para importar e **`export`** para exportar, em vez do `require/module.exports` do CommonJS.<br>
O **ECMAScript Module (ESM)** é o **padrão oficial do JavaScript** para organizar código em múltiplos arquivos.

**_OBS:_** <i><span style="color:red">Por padrão, arquivos <b>`.js`</b> no Node ainda são tratados como <b>`CommonJS`</b>
	e para utilizar o padrão ECMAScript (JavaScript moderno). <b>são necessários seguir algumas regrinhas</b>:</span></i><br>
***regras 1***:. 
	_Definir `"type": "module"` no `package.json`(assim todo arquivo `.js` será interpretado como ESM.)_ 

 ***regra 2:.*** 
	 _Usar como extensão para seus arquivos **`.js`** a extensão **`.mjs`** nos arquivos._


O **ES Modules (ESM)** tem várias formas de **exportar** e **importar** código.
Mais detalhes de todas as opções com exemplos práticos. 
<big><span style="color: blue">./code/ESModule(import-export) (ESM)</span></big>


**Sintaxes:**

```js

// Import
import fs from "fs";

// Export
export function soma(a, b) { return a + b; }

// OBS:. Lembrando que para se trabalhar em NODEJS, é necessário ativação ECMAScript(ESM) no projeto. Pois por "default" (ESM) só funciona no Browser.
```
<br>

**Resumo:**

- **ESM (import/export)** → padrão moderno, compatível com navegador e recomendado para projetos novos.

