# Object JS

Em JavaScript, **object** são coleções de pares "chave: valor" usados para armazenar dados e funcionalidades. Eles são um dos principais tipos de dados e formam a base de muitas estruturas no JavaScript.

Existem várias formas de criar objetos em JavaScript:

## Criando Object

1. **Usando a notação de objeto literal**
   É a forma mais comum e direta de criar um objeto.

```js
// javaScript
const obj = { name: "Caneca", type: "Porcelana", Price: 22.5, Model: "AAA" };
```

2. **Usando `new Object()`**
   Essa abordagem é menos comum, mas pode ser usada para criar um objeto dinamicamente.

```js
// javaScript
const obj = new Object();
obj.name = "Caneca";
obj.type = "Porcelana";
obj.price = 22.5;
obj.model = "AAA";
```

3. **Usando uma função construtora**
   Boa opção quando precisamos criar múltiplas instâncias com as mesmas propriedades.

```js
// javaScript
function Product(name, type, price, model) {
  this.name = name;
  this.type = type;
  this.price = price;
  this.model = model;
}

// instanciação função construtora
const product_1 = new Product("caneca", "porcelana", 22.5, "AAA");
```

4. **Usando uma Factory function (função fabrica)**
   Boa alternativa para criar objetos sem precisar de `new`.

```js
// javaScript
function productCreate (name, type, price, model) {
return name, type, price, model;
}

// exec função fabrica (factory function)
const product_1 = productCreate("caneca", "porcelana", 22.50, "AAA);
```

5. **Usando classes (ES6+)**
   Forma moderna e recomendada para criar objetos com comportamento estruturado.

```js
// javaScript
class Product {
  constructor(name, type, price, model) {
    this.name = nome;
    this.type = type;
    this.price = price;
    this.model = model;
}
```

6. **Utilizando Object.create() - _protótipo_**
   Cria um novo objeto com um protótipo existente.

```js
// javaScritp
const product = {
  name: "caneca",
  type: "porcelana",
  price: 22.5,
  model: "AAA",
};

const newProduct = Object.create(product);

console.log(newProduct.name); // return "caneca"
```

7. **Usando JSON (JavaScript Object Notation)**
   Útil para converter objetos em strings e vice-versa.

```js
// javaScript
const jsonString =
  '{"name": "caneca", "type": "porcelana", "price": "22.50", "model": "AAA" }';

const product = JSON.parse(jsonString); // converte json em Objecto
```

---

---

## Acessando Object JS {}

Em JavaScript, você pode acessar valores de um objeto basicamente de dois jeitos principais:

- `obj.propriedade` (notação de ponto) → quando você sabe o nome certinho.
- `obj["propriedade"]` (notação colchetes)→ quando o nome vem de uma variável ou precisa de mais flexibilidade.

1. \*\*Notação de ponto ( . )
   - Quando você sabe o nome da propriedade e ele é um identificador válido (sem espaços, símbolos, etc).

```js
const pessoa = { nome: "Alex", idade: 50 };
console.log(pessoa.nome); // "Alex"
console.log(pessoa.idade); // 50
```

2. \*\*Notação de colchetes ( [ ] )
   - Quando você quer usar uma variável como chave ou quando a chave tem espaços, números, ou caracteres especiais.

```js
const pessoa = { "nome completo": "Ale Brito", idade: 50 };
console.log(pessoa["nome completo"]); // "Alex Brito"

const chave = "idade";
console.log(pessoa[chave]); // 25
```

## Acessando Object JS Aninhados {..{}}

Quando você tem **objetos dentro de objetos** (propriedades aninhadas), você acessa cada nível **um de cada vez**:

```js
const usuario = {
  nome: "Carlos",
  endereco: {
    rua: "Rua das Flores",
    cidade: "São Paulo",
    cep: "12345-678",
  },
};

// Usando notação de ponto
console.log(usuario.endereco.cidade); // "São Paulo"

// Usando notação de colchetes
console.log(usuario["endereco"]["rua"]); // "Rua das Flores"

// COmbinação de Notação de ponto / colchetes
const chaveEndereco = "endereco";
console.log(usuario[chaveEndereco].cep); // "12345-678"
```

> **_<span style="color: red"><b>ALERTA!</b></span>_** Se alguma das propriedade do meio **não existir**, o acesso pode dar erro!

```js
console.log(usuario.contato.telefone);
// ERRO! Porque `contato` não existe no objeto.

//Para evitar isso, você pode usar o **operador de encadeamento opcional** (`?.`)
console.log(usuario.contato?.telefone);
// undefined (não erro) porque ele para se 'contato' não existir
```

---

---

## Manipulando Object

1. **Adicionando ou modificando object**

```js
// javaScript
const obj = { name: "Alex", lastname: "Brito", heigth: 1.79 };
obj.age = 49;

console.log(obj); // {name: "Alex", age: 49}
console.log(obj.name); // Alex
console.log(obj["name"]); // Alex
```

2. **Removendo propriedades de object**

```js
// javaScript
delete obj.heigth;

console.log(obj); // {name: "Alex", lastname: "Brito", age: 49}
```

3. **Percorrer um object**

```js
// javaScritp
for (let chave in obj) {
  console.log(chave); // return obj key
  console.log(chave, obj[chave]); //  return obj key, obj value
}
```

4. **Obtendo key(chave) e value(valor) de object**

```js
// javaScript
console.log(Object.keys(obj)); // return keys(chaves) name, type, price, model;
console.log(Object.values(obj)); // return value (valor) canec
```

---

---

## Copiando Object

No JavaScript, existem diferentes formas de copiar objetos, dependendo se você precisa de uma cópia **superficial (shallow copy)** ou **profunda (deep copy)**.
<br>

#### Cópia Superficial (Shallow Copy)

Uma cópia superficial copia apenas os valores do primeiro nível do objeto. Se houver objetos aninhados, eles ainda serão referenciados.

1. **Object assign()**
   🔹**Limitação:** Não copia objetos aninhados, apenas o primeiro nível.

```js
// javaScript
const obj = { a: 1, b: 2 };
const copy = Object.assign({}, obj);

copy.a = 99;
console.log(obj.a); // Continua 1 (cópia independente)
```

2. **Spreed Operator (...)**
   🔹 **Limitação:** Objetos aninhados não são copiados, apenas referenciados.

```js
// javaScritp
const obj = { a: 1, b: { c: 2 } };
const copy = { ...obj };

copy.b.c = 99;
console.log(obj.b.c); // 99 (a referência foi mantida)
```

---

---

## Cópia Profunda (Deep Copy)

Uma cópia profunda cria uma cópia real de **todos os níveis do objeto**, garantindo que não haja referências compartilhadas.

1.  `JSON.parse(JSON.stringify(obj))` **(Método Simples)**
    🔹 **Limitação:** Não copia funções, `undefined`, `Symbol`, `Date`, `Map`, `Set`, etc.

```js
// javaScritp
const obj = { a: 1, b: { c: 2 } };
const copy = JSON.parse(JSON.stringify(obj));

copy.b.c = 99;
console.log(obj.b.c); // 2 (o original não foi afetado)
```

2. **`structuredClone(obj)` (Método Moderno)**
   Disponível no **Node.js 17+** e **navegadores modernos**.
   🔹**Vantagem:** Suporta `Date`, `Array`, `Map`, `Set` e valores especiais.  
   🔹 **Limitação:** Não suporta funções.

```js
// javaScript
const obj = { a: 1, b: { c: 2 }, d: new Date() };
const copy = structuredClone(obj);

copy.b.c = 99;
console.log(obj.b.c); // 2 (o original não foi afetado)
console.log(copy.d === obj.d); // false (a data foi clonada)
```

3. Método Recursivo **(Total Controle)**
   Se precisar copiar tudo, incluindo funções.
   🔹**Vantagem:** Copia tudo, inclusive objetos aninhados, `Date` e arrays.  
   🔹**Limitação:** Não copia funções nem `Map`/`Set` (pode ser adaptado).

```js
// javaScript
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;

  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map(deepClone);

  const copy = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      copy[key] = deepClone(obj[key]);
    }
  }
  return copy;
}

const obj = { a: 1, b: { c: 2 }, d: new Date(), e: [1, 2, 3] };
const copy = deepClone(obj);
console.log(copy);
```

---

---

## Method for Object (uteis)

- **Method entries()** => itera com o 'obj', retornando um array para com as chave(kes) e 'value' do 'obj iterado'.

```js
const obj = { name: "Alex", age: 49, gender: "M" };

console.log(Object.entries(obj));
// return [["name", "Alex"], ["age", "49"], ["gender", "M"]]
```

- **Method fromEntries()** => itera com o array retornado pelo 'entries()', retornando um 'obj' com as chaves e valores do array.

```js
console.log(Object.fromEntries(Object.entries(obj)));
// return {name: "Alex", age: 49, gender: "M"}
```

- **Method keys()** => itera com o 'obj', retornando um array com as chaves do 'obj' iterado.

```js
console.log(Object.keys(obj));
// return ["name", "age", "gender"]
```

- **Method values()** => itera com o 'obj', retornando um array com os valores do 'obj' iterado.

```js
console.log(Object.values(obj));
// return ["Alex", 49, "M"]
```

- **Method assign()** => cria/ copia um 'obj' com as propriedades de 'object' referênciado.

```js
const newObj = Object.assign({}, obj);
console.log(newObj);
// return {name: "Alex", age: 49, gender: "M"}

const newObj2 = Object.assign({ heigth: 1.79 }, obj);
console.log(newObj2);
// return {heigth: 1.79, name: "Alex", age: 49, gender: "M"}
```

- **Method create()** => cria um novo 'obj' com as propriedades **(PROTOTYPE)** de 'obj' referênciado.

```js
const newObj = Object.create(obj);
console.log(newObj.name);
// return "Alex"
```

---

---

## Definição de Propriedade

**Descriptor Property** - Uma propriedade em Javascript consiste de um nome com formato _texto-valor_ e um **descritor de propriedades.**

Atributos descritor de propriedade:

[`value`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#value)
o valor associado com a propriedade (somente para descritores de dados).

[**`writable`**](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#writable)
`true` se, e somente se, o valor associado com a propriedade pode ser alterado (somente para descritores de dados).

[`get`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#get)
Uma função que serve como um _getter_, para obter o valor da propriedade, ou [`undefined`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/undefined) se não houver (somente para descritores de acesso).

[`set`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#set)
Uma função que serve como um s*etter*, para atribuir um valor à propriedade, ou [`undefined`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/undefined) se não houver (somente para descritores de acesso).

[`configurable`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#configurable)
`true` se, e somente se, o tipo deste descritor de propriedade pode ser alterado e se a propriedade pode ser excluída do objeto correspondente.

[`enumerable`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor#enumerable)
`true` se, e somente se, esta propriedade aparece durante a enumeração das propriedades do objeto correspondente.

<br>

- **Method defineProperties()** => define ou inclui propriedade**S** para o 'obj' referênciado.

```js
Object.defineProperties(obj, {
	weigth: { value: 119, writable: true, enumerable: true},
	heigth: { value: 1.79, writable: true, enumerable: true},
	...
});

console.log(obj);
// return {name: "Alex", age: 49, gender: "M", weigth: 119, heigth: 1.79}
```

- **Method defineProperty()** => define ou inclui **uma** propriedade para o 'obj' referênciado.

```js
Object.defineProperty(obj, "weigth", {
  value: 119,
  writable: true,
  enumerable: true,
});

console.log(obj);
// return {name: "Alex", age: "1.79, gender: "M", weigth: 119}
```

- **Method freeze()** => congela o 'obj', impedindo que suas propriedades sejam alteradas.

```js
Object.freeze(obj);
```

- **Method seal()** => sela o 'obj', impedindo que suas propriedades sejam deletadas.

```js
Object.seal(obj);
```

- **Method is()** => verifica se dois valores são iguais.

```js
console.log(Object.is(10, 10)); // true
console.log(Object.is(10, "10")); // false
```

- **Method isExtensible()** => verifica se um objeto pode ser extendido (se é ou não possível adicinar novas propriedades).

```js
console.log(Object.isExtensible(obj));
```

- **Method isFrozen()** => verifica se o 'obj' está congelado. Um objeto estará frozen se, e apenas se, ele não for [extensible](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible), todas as suas propriedades não são configuráveis e todas suas propriedades de dados (propriedades que não são asessores de propriedades com getters ou setters) não podem ser modificadas.

```js
console.log(Object.isFrozen(obj));
```

- **Method isSealed()** => // verifica se o 'obj' está selado. Um objeto está selado se ele for "não [extensible](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)" e se todas as suas propriedades estão como "não configuráveis" e assim sendo "não removíveis" (mas não necessariamente "não escrevíveis").

```js
console.log(Object.isSealed(obj));
```

- **Method preventExtensions()** => previne que o 'obj' seja extendido. impede que novas propriedades sejam adicionadas a um objeto (isto é, impede futuras extensões ao objeto).

```js
Object.preventExtensions(obj);
```

- **Method getOwnPropertyDescriptors()** => retorna um objeto com as propriedade**S** do 'obj'. Este método permite uma análise da descrição precisa **das** propriedade do obj de referência.

```js
console.log(Object.getOwnPropertyDescriptors(obj));
/*return {
name:{value: 'Alex', writable: true, enumerable: true, configurable: true},
lastname:{value: 'Brito', writable: true, enumerable: true, configurable: true},
heigth:{ value: 1.79, writable: true, enumerable: true, configurable: true },
age: { value: 49, writable: true, enumerable: true, configurable: true }
}
```

- **Method getOwnPropertyDescriptor()** => retorna um objeto com a propriedade referênciada do 'obj', permitindo uma análise da descrição precisa **da** propriedade do 'obj'.

```js
console.log(Object.getOwnPropertyDescriptor(obj, "name"));
// return {value: 'Alex', writable: true, enumerable: true, configurable: true}
```

- **Method getOwnPropertyNames()** =>retorna um array com todas as propriedades (enumeráveis ou não) encontradas diretamente em um dado objeto.

```js
console.log(Object.getOwnPropertyNames(obj));
// return [ 'name', 'lastname', 'heigth', 'age' ]
```

- **Method getOwnPropertySymbols()** => rretorna uma array com todas propriedades de símbolo encontradas diretamente em um determinado objeto dado.

```js
console.log(Object.getOwnPropertySymbols(obj));
// return [] => não existe symbols no obj referênciado
```

- **Method getPrototypeOf()** => retorna o prototype (isto é, o valor da propriedade interna `[[Prototype]]`) do objeto especificado.

```js
console.log(Object.getPrototypeOf(obj));
//return [Object: null prototype] {}
```

- **Method hasOwnProperty()** => retorna um booleano indicando se o objeto possui a propriedade especificada como uma propriedade definida no próprio objeto em questão (ao contrário de uma propriedade herdada).

```js
console.log(obj.hasOwnProperty("name"));
// return true
```

- **Method propertyIsEnumerable()** => retorna um booleano indicando quando a propriedade especificada é enumerável e é a propriedade do próprio objeto

```js
console.log(obj.propertyIsEnumerable("name"));
// return true
```

- **Method toLocaleString()** => retorna uma cadeia de caracteres (_string_) representando o objeto. Este método é feito para ser sobrescrito por objetos derivados para propósitos de localização específica.
  - **Objetos sobrescrevendo toLocaleString**
    1. [`Array`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array): [`Array.prototype.toLocaleString()`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/toLocaleString)
    2. [`Number`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Number): [`Number.prototype.toLocaleString()`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString)
    3. [`Date`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Date): [`Date.prototype.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString)
    4. [`TypedArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray): [`TypedArray.prototype.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/toLocaleString)
    5. [`BigInt`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/BigInt): [`BigInt.prototype.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt/toLocaleString)

```js
console.log(obj.toLocaleString());
// return Alex,Brito,1.79,49
```

- **Method toString()** => retorna uma string com as propriedades do 'obj'.

```js
console.log(obj.toString());
// return [object Object]
```

- **Method valueOf()** => retorna o valor primitivo do 'obj' especificado. Por padrão, o método **`valueOf`** é herdado por cada objeto descendente de [`Object`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object). Todo núcleo embutido do objeto sobrescreve esse método para retornar um valor apropriado. Se um objeto não tem um valor primitivo, `valueOf` retorna o próprio objeto.

```js
console.log(obj.valueOf());
// return { name: 'Alex', lastname: 'Brito', heigth: 1.79, age: 49 }
```

- **Method hasOwn()** => O método estático **`Object.hasOwn()`** retorna **`true`** se o objeto específicado tem a propriedade indicada como sua propriedade _own_. Se a propriedade é herdada, ou não existe, o método retorna **`false`**.

```js
console.log(Object.hasOwn(obj, "name"));
// return true
```

- **Método `set()`** => O método set() não é um método nativo de objetos JavaScript comuns, mas é usado em algumas estruturas de dados específicas, como **`Map`** e **`WeakMap`**.

```js
// 1 - set() in Map
// Se `obj` for uma instância de `Map`, então `set()` adiciona um par chave-valor.

const obj = new Map();
obj.set("color", "White");
obj.set("size", "Large");

console.log(obj);
// Map(2) { 'color' => 'White', 'size' => 'Large' }

console.log(obj.get("color")); // "White"

/* Explicação:
- `set(chave, valor)` adiciona ou atualiza uma entrada no `Map`.
- `get(chave)` recupera o valor associado à chave. */

// 2 - set() in Objetos Comuns {}
// Objetos JavaScript padrão não têm set().
const obj = {};
obj.set("color", "White");
// Erro: obj.set is not a function

// 3 - Criando um set() personalizado para Objetos
const obj = {
  set: function (key, value) {
    this[key] = value;
  },
};

obj.set("color", "White");
obj.set("size", "Large");

console.log(obj);
// { set: [Function: set], color: 'White', size: 'Large' }

// 4 - set() em `Set` (Conjunto)
// Se `obj` for um `Set` (estrutura de dados para armazenar valores únicos)
const obj = new Set();
obj.add("White");
obj.add("Black");

console.log(obj);
// Set(2) { 'White', 'Black' }
```

- **Method get()** => O método **`.get()`** pertence à estrutura de dados **`Map`** do JavaScript, que é usada para armazenar pares chave-valor, semelhante a um objeto, mas com algumas vantagens, como a possibilidade de usar qualquer tipo de dado como chave.

```js
// Criando um Map
const usuarios = new Map();

// Adicionando elementos com set()
usuarios.set("123", { nome: "João", idade: 30 });
usuarios.set("456", { nome: "Maria", idade: 25 });

// Usando get() para acessar valores
console.log(usuarios.get("123")); // { nome: "João", idade: 30 }
console.log(usuarios.get("456").nome); // "Maria"

// Tentando acessar uma chave inexistente
console.log(usuarios.get("789")); // undefined


// Diferenças entre `Map` e `Object`

/*
No `Map`, as chaves podem ser "qualquer tipo de dado" (objetos, números, funções, etc.), enquanto no `Object`, elas são sempre strings ou símbolos.
`Map` mantém a ordem de inserção das chaves.
`Map` tem métodos úteis como `.size`, `.has()`, `.delete()`, além do `.get()`, enquanto `Object` exige mais verificações manuais.
```

- Method clear() => O método `clear()` **não existe para objetos comuns (`{}`)** em JavaScript. Porém, ele está disponível para **`Map` e `Set`**, deleta todas as propriedades do 'obj'.

```js
// 1 - clear() em Map
// Se obj for um Map, o método clear() remove todas as entradas
const obj = new Map();
obj.set("color", "White");
obj.set("size", "Large");
console.log(obj);
// Map(2) { 'color' => 'White', 'size' => 'Large' }

obj.clear(); // Remove todos os pares chave-valor
console.log(obj); // Map(0) {}

// 2 - clear() em Set
// Se obj for um Set, clear() remove todos os elementos
const obj = new Set(["Apple", "Banana", "Orange"]);
console.log(obj); // Set(3) { 'Apple', 'Banana', 'Orange' }

obj.clear(); // Remove todos os elementos
console.log(obj);
// Set(0) {}

// 3 - Como limpar um objeto {}
// Objetos comuns não têm clear(), mas você pode esvaziá-los manualmente

// redefinir object
let obj = { name: "Alice", age: 25 };
obj = {}; // Cria um novo objeto vazio
console.log(obj); // {}

// Remover todas as propriedades Object.keys()
const obj = { name: "Alice", age: 25 };
Object.keys(obj).forEach((key) => delete obj[key]);
console.log(obj); // {}

// Usando Object.assign()
const obj = { name: "Alice", age: 25 };
Object.assign(obj, {}); // Copia um objeto vazio para 'obj'
console.log(obj); // {}
```

- **Method forEach()** => O método **`forEach`** não funciona diretamente em objetos. O método **`forEach`** é usado em **arrays**, não em objetos. Se `obj` for um objeto, você precisará usar **`Object.entries(obj)`, `Object.keys(obj)` ou `Object.values(obj)`** para iterar sobre ele.

```js
Object.entries(obj).forEach(([key, value]) => console.log(key, value));
/* return
name Alex
lastname Brito
heigth 1.79
age 49

Explicação:
Object.entries(obj) // return um array de pares [chave, valor], permitindo iterar sobre ele com `forEach`.
[key, value] é uma desestruturação do array [chave, valor] para obter os elementos diretamente. */*


// Se você quiser apenas as **chaves**, use Object.keys(obj)
Object.keys(obj).forEach(key => console.log(key, obj[key]));


// Se precisar apenas dos **valores**, use Object.values(obj)
Object.values(obj).forEach(value => console.log(value));
```

- Method size() => Em JavaScript, **`Object`** não possui um método **`.size()`** como o `Map`, mas você pode obter o tamanho (quantidade de chaves) de um objeto de outras maneiras.

```js
// obtendo o tamanho de um `Object` usando `Object.keys().length`
const usuario = { nome: "João", idade: 30, cidade: "São Paulo" };
console.log(Object.keys(usuario).length); // 3

// `Object.keys(obj)` retorna um array com todas as chaves do objeto, e `.length` conta quantos elementos existem.

// Usando `Object.entries().length`
console.log(Object.entries(usuario).length); // 3

// `Object.entries(obj)` retorna um array de pares `[chave, valor]`, e `.length` conta os elementos.

// Criando um método `size()` para `Object`
Object.prototype.size = function () {
  return Object.keys(this).length;
};

console.log(usuario.size()); // 3

// **Cuidado!** Modificar `Object.prototype` pode causar conflitos inesperados.

// com `Map`
const map = new Map([
  ["nome", "João"],
  ["idade", 30],
  ["cidade", "São Paulo"],
]);

console.log(map.size); // 3

// `Map` tem `.size` nativo, enquanto `Object` exige `Object.keys(obj).length`.
```

---

---

## Object Map()

O objeto `Map` no JavaScript é uma estrutura de dados que permite armazenar pares **chave-valor**, assim como um objeto (`{}`), mas com algumas vantagens:

### Principais características do `Map`:

- **As chaves podem ser de qualquer tipo**, incluindo objetos, funções e valores primitivos.
- **Mantém a ordem de inserção** dos elementos, diferente de um objeto, que não garante ordem nas chaves.
- **Possui métodos específicos** para manipular os dados de forma eficiente.

🔹 **CRIANDO `Map()`**

```js
const myMap = new Map();
```

<br>

🔹 **ADCIONANDO VALORES (`set`)**

```js
myMap.set("name", "Alex");
myMap.set(42, "Número como uma chave");
myMap.set({ id: 1 }, "Objeto como uma chave");
```

<br>
🔹 **OBTENDO VALORES (`get`)**

```js
console.log(myMap.get("nome")); // João
console.log(myMap.get(42)); // Número como chave
```

<br>
🔹 **VERIFICANDO SE UMA CHAVE EXISTE (`has`)**

```js
console.log(meuMapa.has("nome")); // true
console.log(meuMapa.has("idade")); // false
```

<br>
🔹 **REMOVENDO UM ITEM (`delete`)**

```js
meuMapa.delete(42);
console.log(meuMapa.has(42)); // false
```

<br>
🔹 **OBTENDO O TAMANHO (`size`)**

```js
console.log(meuMapa.size); // 2 (porque removemos uma entrada)
```

<br>
🔹 **ITERANDO SOBRE O `Map`**

```js
// Percorrendo chaves e valores
meuMapa.forEach((valor, chave) => {
  console.log(`Chave: ${chave}, Valor: ${valor}`);
});

// Ou usando for...of
for (let [chave, valor] of meuMapa) {
  console.log(`Chave: ${chave}, Valor: ${valor}`);
}
```

<br>
🔹 **OBTENDO TODAS AS CHAVES OU VALORES (`keys()`, `values()`)**

```js
console.log([...meuMapa.keys()]); // ["nome", { id: 1 }]
console.log([...meuMapa.values()]); // ["João", "Objeto como chave"]
```

<br>
🔹 **LIMPANDO O `Map` (`clear`)**

```js
meuMapa.clear();
console.log(meuMapa.size); // 0
```

<br>
**Quando Usar `Map` ao Invés de um Objeto `{}`?**

| `Map`                                         | `Object`                                 |
| --------------------------------------------- | ---------------------------------------- |
| Qualquer tipo pode ser chave                  | Apenas strings e símbolos como chave     |
| Mantém a ordem de inserção                    | Ordem não garantida                      |
| Métodos práticos (`set`, `get`, `size`, etc.) | Precisa de manipulação manual            |
| Melhor performance ao lidar com muitos itens  | Pode ser mais lento em algumas operações |
