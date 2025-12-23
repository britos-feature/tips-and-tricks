[[#CommonJS (Export - Import)|CommonJS (import, export padrão NodeJS)]]
[[#ESM - ECMAScript Modules (Export - Import)| ECMAScript (import, export padrão JS moderno)]]

# Introdução

Node.js é um ambiente de execução para **JavaScript no lado do servidor**. Ele permite que você execute código JavaScript fora do navegador, o que é útil para criar servidores, APIs, aplicações web e muito mais.

#### Principais características:

- **Baseado no V8** → Usa o motor JavaScript V8 do Chrome, tornando a execução do código rápida.

- **Assíncrono e não bloqueante** → Usa um modelo baseado em eventos, o que melhora o desempenho em operações de I/O (entrada e saída), como acessar banco de dados e arquivos.

- **Gerenciador de pacotes (NPM)** → Possui um ecossistema rico de pacotes e módulos que facilitam o desenvolvimento.

#### Funcionamento básico do NodeJS

- Sistemas de módulos: **CommonJS vs ES Modules**

	Node.js suporta dois tipos de módulos
	- **CommonJS (CJS)** → `require` e `module.exports` (padrão no Node.js)
	- **ECMAScript Modules (ESM)** → `import` e `export` (mais moderno)


## CommonJS (Export - Import)
#importNodejs
O **CommonJS** é o sistema de módulos padrão no Node.js até hoje. Ele usa `require()` para importar módulos e `module.exports` para exportar.

#### Export default (padrão) - ==CommonJS==

```js
// calculator.js 
// CommonJS module.exports
function sum (a, b) {
    return a + b;
}

module.exports = sum; // (MODULE.EXPORTS)
// Exporta apenas uma função


// main.js 
// CommonJS import
const sum = require("./calculator");
sum(2, 5); // return 7

//---------------------------------------------------

// calculator.js
// CommonJS exports
function sum (a, b) {
    return a + b;
};

exports.calculator = sum; // (EXPORTS)


// main.js
// CommonJS import (default)
const calculator = require("./calculator");
calculator.sum(2, 5); // return 7

// or 

// CommonJS import destructuring (destruturação)
const { sum } = require("./calculator");
sum(2, 5); // return 7
```


#### Multiple export - ==CommonJS==

```js
// calculator.js
// CommonJS export multiple 
const sum = ((a, b) => a + b); // mais
const fewer = ((a,b) => a - b); // menos


module.exports = { sum, fewer };
// Exporta multiplos objects


// main.js
// CommonJS multiple import
const calculator = require("./calculator");

calculator.sum(2, 5); // return 7
calculator.fewer(5, 2); // return 3


// CommonJS multiple import destructuring (destruturação)
const { sum, fewer } = require("./calculator");

sum(2, 5); // return 7
fewer(5, 2); // return 3
```


## ESM - ECMAScript Modules (Export - Import) 
#importJsmoderno

O padrão **ES Modules** (`import/export`) é mais moderno e permite melhor suporte para o **JavaScript moderno**.

**Para ativar o suporte a ES Modules no Node.js**, você precisa:

1. Adicionar `"type": "module"` no **package.json**.
2. Usar arquivos `.mjs` ou `import` e `export` diretamente.

#### Export default (padrão) - ==ESM ECMAScript Modules 
- Permite exportar um único valor padrão do módulo.
- Geralmente usado para exportar a principal funcionalidade do módulo **exportação (única)**

```js
// person.js
const name = "Jonh",
const age = 33;

function greet() {
	return `Olá ${name} de ${age} anos!`;
}


// ESM export default (padrão) para ** FUNCTION **
// Um módulo só pode ter uma `export default`, que pode ser importado sem chaves `{}`.
export default greet; // export function



// ESM export default (padrão) para ** VARIAVEIS **
// Lembrando que um módulo só pode ter uma `export default`, que pode ser importado sem chaves `{}`.
export default const name = "Jonh"; // ERROR - Modo de exportação inválido para ** VARIÁVEL **

const name = "jonh"; // CORRETO - Modo de exportação correto para ** VARIÁVEL. **
export { name as default };
```



#### ==Import== default (padrão) ==ESM ECMAScript Modules==

```js
// main.js
// OBS. Na importação default (padrão) ESM - pode-se utiliza-se de qualquer "nome" para referênciar-se ao modulo exportado.
import saudacao from "./person.js";

saudacao(); // return "Olá Jonh de 33 anos!"
```


#### Explicit ==export ESM ECMAScript Modules==

```js
// person.js
// ESM Export variables
export let num1 = 1, num2 = 20, num3; // também var, const

// ESM Export function  ESM 
export function greet() {return "Hello World!"};

// ESM Export class
export class People {
	construct(){
	this.name = "Alex";
	this.age = 49;
	}
}
```


#### Explicit ==IMPORT==  all (tudo) ==ESM ECMAScript Modules==

```js
import * as atributtes from "./person.js";

atributtes.num2; // return 20
atributtes.greet(); // return "Hello World!"

const person = new atributtes.People();
person.name; // return "Alex"
```


#### Explicit ==IMPORT==  list seletion  ==ESM ECMAScript Modules==

```js
// ESM IMPORT LIST, permite importar itens específicos de um módulo usando seus proprios nomes. (possivel renomer no import utilizando "as")
import { People, greet, num2 as number } from "./person.js";

const person = new People();
person.name; // return "Alex"

greet(); // return "Hello World!"

number; // antigo num2, renomeado para number - return 20
```


#### Explicit ==Export== list mista ==ESM ECMAScript Modules==

```js
// module.js

// ESM EXPORT list mista permite export itens especificos e default de um módulo juntamente.

const name = "Alex";
let lastname = "Brito";

function greet (name, lastname) {
	return `Welcome ${name} ${lastname}`;
}

class People {
	constructor (n, l) {
		this.name = n;
		this.lastname = l;
	}
	
	greet () {
		return `Hello, ${this.name} ${this.lastname}`
	}
}  

export { People as default, name, lastname, greet };
```


#### Explict ==Import== list mista ==ESM ECMAScript Modules==

```js
//  ESM Import list mista permite impotação itens especificos e default de um módulo juntamente.
import People, * as module from "./module.js"

const person = new Peole("Maria", "Eduarda");
person.name; // Maria
person.lastname; // Eduarda

module.name; // Alex
module.lastname; // Brito
module.greet(module.name, module.lastname); // Alex Brito
```


#### Dinamic IMPORT  ==ESM ECMAScript Modules==
- A função **`import()`** pode ser chamada como uma função para importar módulos dinamicamente.
- Isso é particularmente útil para carregar módulos sob demanda ou em cenários de carregamento lento.

```js
import("./person.js").then((itens) => {
	const person = new itens.People();
	person.name; // return "Alex"

	itens.greet(); // return "Hello World!"

	itens.num2; // return 20
});
```

