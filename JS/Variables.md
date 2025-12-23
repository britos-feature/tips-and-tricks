# Variable
Em JavaScript, **VARIÁVEIS** são usadas para armazenar valores, como números, textos, objetos e muito mais. Elas são declaradas usando **var, let ou const**.

### **Tipos de Variáveis no JavaScript**

***Declaração de Variáveis***

- **`var`** (Evite usar) → Tem escopo global ou de **função() {}** e pode ser redeclarada.
- **`let`** (Mais recomendada) → Tem escopo de **bloco {}** e pode ser reatribuída.
- **`const`** (Para valores fixos) → Tem escopo de bloco e **não pode ser reatribuída**.

### **Tipos de Dados no JavaScript**

#### **Primitivos** (Armazenam valores simples)
Em JavaScript, **valores primitivos** são tipos de dados que não são objetos e não possuem métodos próprios. Eles são imutáveis, ou seja, não podem ser alterados diretamente.

- **String** → Texto (`"Olá, mundo!"`, `'JavaScript'`)
- **Number** → Números inteiros ou decimais (`42`, `3.14`)
- **Boolean** → Verdadeiro ou falso (`true`, `false`)
- **Undefined** → Variável declarada sem valor (`let x;`)
- **Null** → Representa ausência de valor (`let y = null;`)
- **BigInt** → Para números grandes (`BigInt(12345678901234567890)`)
- **Symbol** → Identificadores únicos (`Symbol("id")`)

#### **Symbol()**
 Symbol()  é um **tipo primitivo** em JavaScript usado para criar **valores únicos**. Ele é útil quando queremos **identificadores exclusivos** para propriedades de objetos, evitando conflitos com outras propriedades.

| Método                              | Descrição                                   |
| ----------------------------------- | ------------------------------------------- |
| `Symbol("descricao")`               | Cria um símbolo único                       |
| `Symbol.for("chave")`               | Cria ou obtém um símbolo global             |
| `Symbol.keyFor(symbol)`             | Obtém a chave de um símbolo global          |
| `Object.getOwnPropertySymbols(obj)` | Obtém as propriedades `Symbol` de um objeto |

O `Symbol` é útil para **proteger propriedades**, criar **identificadores exclusivos** e **evitar colisões** em objetos.

#### **Referência** (Armazenam referências a objetos na memória)

- **Object** → Conjunto de pares chave-valor `{ nome: "Carlos", idade: 28 }`
- **Array** → Lista ordenada de valores `[1, 2, 3, "texto"]`
- **Function** → Função como variável (`function saudacao() {}`)

#### **Diferença entre Tipos Primitivos e de Referência**

- **Primitivos** são armazenados diretamente na memória e comparados por valor.
- **Referência** armazena um **endereço na memória**, e não o valor diretamente.

```JavaScript
// JavaScritp

let a = 10;
let b = a;
b = 20;
console.log(a); // 10 (valor original não muda)

let obj1 = { nome: "Pedro" };
let obj2 = obj1;
obj2.nome = "Lucas";
console.log(obj1.nome); // "Lucas" (muda pois ambos apontam para o mesmo objeto)
```

Se quiser copiar um objeto sem referência, use **spread operator** (`...`):

```js
// JavaScritp

let obj1 = { nome: "Ana" };
let obj2 = { ...obj1 };
obj2.nome = "João";
console.log(obj1.nome); // "Ana" (obj1 não foi alterado)

```


### Navegadores e JavaScript
Variáveis e funções definidas em no **`JS`**, não estão acessíveis no console do navegador porque estão no escopo do módulo **(por padrão, todo script importado é tratado como um módulo em navegadores modernos)**. 

Para acessá-las no console do navegador, há algumas soluções:

#### Definir explicitamente no `window` -  (melhor prática)
Se você quiser tornar **`variáveis`** e **`funções`** acessíveis globalmente, pode adicioná-los ao objeto **`window`**.
Dessa forma, você pode acessar as variáveis e funções no console do navegador sem problemas. 

```js
// file.js

window.name = "Alex"; // variable
window.greet = (name) => `Hello ${name}, welcome!`; // arrow-fucntion
```

Agora, no console do navegador, você pode digitar

```js
greet(name)  // "Hello Alex, welcome!"
```


#### Remover o escopo do módulo
Por padrão, os **`scripts`** são tratados como MODULES em alguns casos. Para garantir que o código esteja no escopo global, altere a inclusão do script no `index.html`.

```html
<script src="file.js" defer></script>
```

> **Evite adicionar `type="module"`** (a menos que realmente precise de módulos ES6), pois isso impede que as variáveis sejam globais.


#### Definir variáveis no escopo global (sem `const` ou `let`)
Em JavaScript, variáveis declaradas sem **`const`, `let`** ou **`var`** tornam-se automaticamente propriedades do **`window`**.

```js
// file.js

name = "Alex"; // variable
greet = (name) => `Hello ${name}, welcome!`; // arrow-fucntion
```

Agora, no console do navegador, você pode digitar

```js
greet(name)  // "Hello Alex, welcome!"
```


