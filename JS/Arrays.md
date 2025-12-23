# Array JS

Em JavaScript, um **array** é uma estrutura de dados que armazena uma coleção ordenada de elementos. Os arrays podem conter qualquer tipo de dado, incluindo números, strings, objetos e até outras arrays (arrays aninhados).

> Em JavaScript um array(Vector) é um Object.

### **Criando um Array**

Existem diferentes formas de criar um array em JavaScript:

1. **Usando notação de colchetes (`[]`)**

```js
let numeros = [1, 2, 3, 4, 5];
let frutas = ["Maçã", "Banana", "Laranja"];
```

<br>

2. **Usando o construtor `Array()`**

```js
let myArray = new Array(10); // Cria um array com 10 posições vazias
let nomes = new Array("João", "Maria", "Carlos");
```

<br>

3. **Array dentro de array, colchetes (`[]`)**

```js
let data = [1, 2, 3, 4, 5, ["a", "b", "c", "d", "e"]];
// acessos
console.log(data[3]); // return 4
console.log(data[5][3]); // return "d"

let data1 = [
  [1, 2, 3],
  [10, 20, 30],
  [100, 200, 300],
];
// acessos
console.log(data1[0][1]); // return 2
console.log(data1[1][0]); // return 10
console.log(data1[2][2]); // return 300
```

<br>

4. **Array dentro de array, construtor `Array()`**

```js
let data = new Array(
  1,
  2,
  3,
  4,
  5,
  new Array(10, 20, 30, 40, 50),
  new Array(100, 200, 300, 400, 500)
);

// acessos
console.log(data[1]); // return 2
console.log(data[5][0]); // return 10
console.log(data[6][2]); // return 300
```

<br>

5. **Object dentro de array**

```js
let myArrayObject = [
  { name: "Alex", age: 49, gender: "M" },
  { name: "Maria", age: 8, gender: "F" },
  { name: "Joaquim", age: 5, gender: "M" },
];
// acessos
console.log(myArrayObject[0]);
// return { name:"Alex", age:"49", gender: "M" }

console.log(myArrayObject[1].age); // return 8

console.log(myArrayObject[2]["name"]); // return "Joaquim"
```

---

---

### Acessando elementos do array (modos)

```js
// por index
// Cada elemento do array tem um índice, começando do zero.
let num = [10, 20, 30, 40];
console.log(num[3]);
// return 40

// utilizando loop for basic (percorre o array)
// Percorre o array usando um loop tradicional.
for (let i = 0; i < num.length; i++) {
  console.log(num[i]);
  // return 10, 20, 30, 40
}

// utilizando forEach()
// Método mais moderno para percorrer arrays.
num.forEach((num, index) => {
  console.log(`${index}: ${num}`);
  // return 0: 10, 1: 20, 2: 30, 3: 40
});

// utilizando map() (cria um novo array)
// Retorna um novo array após aplicar uma função em cada elemento.
let newArray = num.map((num) => {
  return num;
});
console.log(newArray);
// return [10, 20, 30, 40]

// utilizando loop for ...of
// Forma mais simples de percorrer arrays.
for (let n of num) {
  console.log(n);
  // return 10, 20, 30, 40
}

// utilizando find() / findIndex()
// find: Retorna o primeiro elemento que atende à condição.
let result = num.find((num) => {
  if (num === 30) return num;
});
console.log(result); // return 30 = valor

// findIndex: Retorna o índice do primeiro elemento que atende à condição.
let result2 = num.findIndex((num) => {
  if (num === 20) return num;
});
console.log(result2); // return 1 = index

// utilizando filter() (cria um novo array)
// Retorna um novo array com todos os elementos que atendem à condição.
let numUp = num.filter((num) => {
  if (num >= 20) return num;
});
console.log(numUp);
// return [20, 30, 40]

// utilizando reduce() (acumulador)
// Acumula valores do array em um único valor.
let numNew = num.reduce((acc, num) => {
  acc += num;
  return acc;
}, 0);
console.log(numNew);
// return 100

// utilizando destruturação
// Extrai valores diretamente do array.
let [, , terceiro] = num;
console.log(terceiro);
// return 30
```

---

---

### Principais Métodos de Arrays

1. **Adicionar e Remover Elementos**

- **`push(item)`** → Adiciona um item ao final do array.
- **`pop()`** → Remove o último item do array.
- **`unshift(item)`** → Adiciona um item no início do array.
- **`shift()`** → Remove o primeiro item do array.
- **`splice()`** -> Adiciona / exclui item no array a parti de um determinado index.
- **`delete`** -> Deleta **um item** no array sem modificar os index do array, deixando o index vazio (empty)

```js
let lista = [10, 20, 30];

listum=ush(40);  // [10, 20, 30, 40]
lista.pop();      // [10, 20, 30]

lista.unshift(5); // [5, 10, 20, 30]
lista.shift();    // [10, 20, 30]

list.splice(2, 0, 50); // [10, 20, 50, 30]

delete list[2]; // [10, 20, 30]
/* Não use `delete` para remover arrays
O comando `delete` não remove um array corretamente, ele apenas define o índice como `undefined`, o que pode causar problemas.
```

<br>

2. **Retornando o TAMANHO do array**.

```js
let names = ["Alex", "Maria", "Joaquim", "Eliane"];
console.log(names.length); // return 4
```

<br>

3. **Verificação se uma variéval é um array**

```js
names intanceof array; // return true
```

<br>

4. **Ordenando array utilizando sort() / reverse()**

```js
/*
IMPORTANTE !!
Ao executar uma ordenação de arrays, automáticamente o mesmo perde a sequencia original dos seus INDEX.
Dica: Crie sempre uma copia do array original antes de ordena-lo, utilizando o
spred Operator (...)
*/

let names = ["Maria", "Joaquim", "Eliane", "Alex", "Vinícius", "Victor"];
let num = [10,30,25,5,1,23,54,13];

let peoples = [
	{name: "Maria", age: 8, gender: "F"},
	{name: "Joaquim", age: 5, gender: "M"},
	{name: "Eliane", age: 38, gender: "F"},
	{name: "Alex", age: 49, gender: "M"},
	{name: "Vinícius", age: 25, gender: "M"},
	{name: "Victor", age: 28, gender: "M"}
];

// Ordenando de (a, z) default
console.log(names.sort());
// return ['Alex', 'Eliane', 'Joaquim', 'Maria', 'Victor', 'Vinícius'] - default


// Ordenando por numeração
console.log(num.sort((n1, n2) => {
	return n1 - n2;
}));
// return [1, 5, 10, 13, 23, 25, 30, 54]


// Ordenando por ordem crescente (number)
console.log(peoples.name.sort(a1, a2) => {
	return a1.age - a2.age
}));
/* return [
	{ name: 'Joaquim', age: 5, gender: 'M' },
	{ name: 'Maria', age: 8, gender: 'F' },
	{ name: 'Vinícius', age: 25, gender: 'M' },
	{ name: 'Victor', age: 28, gender: 'M' },
	{ name: 'Eliane', age: 38, gender: 'F' },
	{ name: 'Alex', age: 49, gender: 'M' }
] */


// Ordenando de (a, z)
console.log(peoples.sort((n1, n2) => {
	return n1.name.localeCompare(n2.name);
}));
/* return [
	{ name: 'Alex', age: 49, gender: 'M' },
	{ name: 'Eliane', age: 38, gender: 'F' },
	{ name: 'Joaquim', age: 5, gender: 'M' },
	{ name: 'Maria', age: 8, gender: 'F' },
	{ name: 'Victor', age: 28, gender: 'M' },
	{ name: 'Vinícius', age: 25, gender: 'M' }
]
```

<br>

5. **Buscar e Filtrar Elementos**

- **`indexOf(item)`** → Retorna a posição do item ou `-1` se não encontrar.
- **`includes(item)`** → Retorna `true` se o item existir no array.
- **`find(callback)`** → Retorna o primeiro elemento que satisfaz a condição.
- **`filter(callback)`** → Retorna um novo array apenas com os elementos que atendem à condição.

```js
let numeros = [10, 20, 30, 40];

console.log(numeros.indexOf(20)); // 1
console.log(numeros.includes(50)); // false

let maiorQue25 = numeros.find((num) => num > 25);
console.log(maiorQue25); // 30

let filtrados = numeros.filter((num) => num > 15);
console.log(filtrados); // [20, 30, 40]
```

<br>

6. **Juntar / concatenar elementos ARRAY**

- **`join()`** -> O método **`join()`** é usado para **converter um array em uma string**, separando os elementos por um delimitador especificado.
  - **`separador`** (opcional): String usada para separar os elementos. Se omitido, o padrão é `","` (vírgula).
- **`concat()`** -> O método `concat()` é usado para **juntar dois ou mais arrays** e retornar um **novo array** (sem modificar os originais).

```js
let names = ["Alex", "Maria", "Joaquim", "Eliane"];
let num = [1, 2, 3];

// join()
console.log(names.join(", "));
// return Alex, Maria, Joaquim, Eliane

// concat() -> Adicionando element em um novo array.
let resultConcat = names.concat("Vinícius", "Victor");
console.log(resultConcat);
// return [ "Alex", "Maria", "Joaquim", "Eliane", "Vinícius", "Victor" ]

// concat() -> juntando 2 arrays ou mais ...
let resultConcat2 = names.concat(num);
console.log(resultConcat2);
// return [ "Alex", "Maria", "Joaquim", "Eliane", 1, 2, 3 ]
```

---

---

#### Remover / Adicionar elemento em uma posição específica no array

1. **`splice()`** - O método `.splice()` em JavaScript é usado para **adicionar, remover ou substituir elementos** em um array. Diferente de `.slice()`, que retorna uma nova cópia, o `.splice()` **modifica o array original**.

**Sintaxe:**
**`splice(start, amount, element1, element2, ...)`**;

- start: O índice onde começar a modificar o array.
- **amount**: O número de elementos a ser removido a partir do índice **início**.
- **element1, element2, ...**: Itens que serão **adicionados** no array a partir do índice **início**. (Opcional)

```js
let numeros = [1, 2, 3, 4, 5];

// Remove elementos (CRIA UM NOVO ARRAY COM ELEMENTOS REMOVIDO)
let removedElements = numeros.splice(2, 1);
// Remove um elemento a partir do index 2 = valor(3)
console.log(removedElements);
// return [1, 2, 4, 5]

// Adcionando elementos (NÃO CRIA UM NOVO ARRAY, ADICIONA OS ELEMENTOS NO ARRAY ORIGINAL)
numeros.splice(2, 0, 55, 66);
// Adciona elementos a partir do index 2
console.log(numeros);
// return [1, 2, 55, 66, 3, 4, 5]
```

<br>

2. **`slice()`** - O método `.slice()` em JavaScript é utilizado para **extrair uma parte de um array ou string** e retornar uma **nova cópia** dessa parte, sem modificar o array ou string original.

**Sintaxe:**
**`slice(start, end)`**;

- **start** (obrigatório): O índice de onde começar a extração (inclusive).
- **end** (opcional): O índice onde a extração termina (não inclusivo). Se omitido, o **slice** vai até o final do array/string.

> Importante!
> O método **`slice()`** não altera o array original.

```js
let num = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// Utilizando slice() sem argumentos, só com o "START"
let resultSlice = num.slice(2);
console.log(resultSlice);
// return [3, 4, 5, 6, 7, 8, 9]

// Utilizando slice() com array
let resultSlice1 = num.slice(1, 3);
console.log(resultSlice1);
// return [ 2, 3, 4]

let name = "Alex Brito";

// Utilizando slice() com string
let resultSlice2 = name.slice(0, name.indexOf(" ")); // name.indexOf(" ") = 8
console.log(resultSlice2);
// return Alex
```

---

---

#### Métodos de array `filter()`, `map()`, `reduce()`, `every()`, `some()`

1. **`filter()`** - O método `.filter()` cria um **novo array** contendo apenas os elementos que passam em um teste (função de callback).

Sintaxe: **`array.filter(callBack(element, index, array), thisArg)`**

- `callback` → Função que testa cada elemento.
  - **`elemento`** → O item atual do array.
  - **`índice`** _(opcional)_ → A posição do item no array.
  - **`array`** _(opcional)_ → O próprio array original.
- **`thisArg`** _(opcional)_ → Valor a ser usado como `this` dentro da função.

```js
let returnAPI = [
  { id: 0, name: "Alex", email: "alex@gmail.com", gender: "M" },
  { id: 1, name: "Maria", email: "maria@ig.com", gender: "F" },
  { id: 2, name: "Joaquim", email: "joaquim@email.net", gender: "M" },
  { id: 3, name: "Eliane", email: "eliane@user.org", gender: "F" },
];

// filter(), filtrando resultado de todos gender "F"
let resultFilter = returnAPI.filter((value) => {
  return value.gender === "F";
});

console.log(resultFilter);
/* return [
	{ id: 1, name: 'Maria', email: 'maria@ig.com', gender: 'F' },
	{ id: 3, name: 'Eliane', email: 'eliane@user.org', gender: 'F' }
] 
*/
```

---

---

2. **`map()`** - o método **`map()`** é usada para criar um novo array aplicando uma função(callBack) dada a cada elemento de um array existente. Ela não modifica o array original, mas retorna um novo array com os resultados.

Sintaxe: \*\*`array.map(callBack(elemento, indice, array))

- **`elemento`**-> O item atual no array que está sendo processado.
- **`índice`** (opcional) -> O índice do item atual.
- **`array`** (opcional) -> O array original que está sendo percorrido.

```js
let returnAPI = [
  { id: 0, name: "Alex", email: "alex@gmail.com", gender: "M" },
  { id: 1, name: "Maria", email: "maria@ig.com", gender: "F" },
  { id: 2, name: "Joaquim", email: "joaquim@email.net", gender: "M" },
  { id: 3, name: "Eliane", email: "eliane@user.org", gender: "F" },
];

// map(), deletando propriedade "gender" para todos elementos gender: "F"
let resultMap = returnAPI.map((value) => {
  return value.gender === "F"
    ? {
        id: value.id,
        name: value.name,
        email: value.email,
      }
    : value;
});

console.log(resultMap);
/* return [
	{ id: 0, name: 'Alex', email: 'alex@gmail.com', gender: 'M' },
	{ id: 1, name: 'Maria', email: 'maria@ig.com' },
	{ id: 2, name: 'Joaquim', email: 'joaquim@email.net', gender: 'M' },
	{ id: 3, name: 'Eliane', email: 'eliane@user.org' }
]
*/
```

---

---

3. **`reduce()`** - O método **`reduce()`** em JavaScript é usado para reduzir um array a um único valor, aplicando uma função (callBack) a cada elemento do array. A função de callback recebe quatro parâmetros:

Sintaxe: \*\*`array.reduce(callBack(acumulator, valueCurrent, index, array), valueInitial)

- **Acumulador**: O valor acumulado retornado pela última invocação do callback. Na primeira chamada, é o valor inicial ou o primeiro elemento do array.
- **Valor Atual**: O elemento atual sendo processado no array.
- **Índice Atual** (opcional): O índice do elemento atual sendo processado.
- **Array** (opcional): O array no qual o `reduce()` foi chamado.

- **`callback`:** A função que será chamada para cada elemento no array.
- **`valorInicial`** (opcional): O valor a ser usado como o primeiro argumento na primeira chamada da função callback. Se não for fornecido, o primeiro elemento do array será usado.

```js
let returnAPI = [
  { id: 0, name: "Alex", email: "alex@gmail.com", gender: "M" },
  { id: 1, name: "Maria", email: "maria@ig.com", gender: "F" },
  { id: 2, name: "Joaquim", email: "joaquim@email.net", gender: "M" },
  { id: 3, name: "Eliane", email: "eliane@user.org", gender: "F" },
  { id: 4, name: "Irmã", email: "irma@irma.net", gender: "F" },
];

// reduce(), retornando a quantidade de todos elementos gender: "F"
let resultReduce = returnAPI.reduce((ac, value) => {
  value.gender === "F" ? (ac += 1) : false;
  return ac;
}, 0);

console.log(`Quantidade de funcionárias: ${resultReduce}`);
// return 3
```

---

---

4. **`every()`** - O método **`every()`** em JavaScript é usado em arrays para verificar se **todos** os elementos atendem a uma determinada condição. Ele retorna **`true`** se **TODOS OS ELEMENTOS DO ARRAY** passarem no teste fornecido por uma **função(callback)**, caso contrário, retorna **`false`**.

Sintaxe: **`array.every(callback(element, index, array), thisArg)`**

> IMPORTANTE! - O método **`every()`** não modifica o array original.

- **callback**: Função que será executada para cada elemento do array.
  - **`element`**: O elemento atual.
  - **`index`** _(opcional)_: O índice do elemento atual.
  - **`array`** _(opcional)_: O array completo.
- **`thisArg`** _(opcional)_: Um valor para ser usado como `this` dentro da função callback.

```js
let returnAPI = [
  { id: 0, name: "Alex", email: "alex@gmail.com", gender: "M" },
  { id: 1, name: "Maria", email: "maria@ig.com" },
  { id: 2, name: "Joaquim", email: "joaquim@email.net", gender: "M" },
  { id: 3, name: "Eliane", email: "eliane@user.org", gender: "F" },
  { id: 4, name: "Irmã", email: "irma@irma.net", gender: "F" },
];

// every(), verificando se todo array atende a condição de "TER" uma propriedade "gender", retornando true para positivo.
let resultEvery = returnAPI.every((value) => {
  // return value.hasOwnProperty("gender"); // condição para testar propriedade
  return "gender" in value;
});

console.log(resultEvery);
// return false
```

---

---

5. **`some()`** - O método **`some`** em JavaScript é usado em arrays para verficação se **ALGUNS DOS ELEMENTOS DO ARRAY** atendem a uma determinada condição passada como teste fornecido por uma **`função(call\back)`** for atendida. Basta um dos elementos do array atender ao teste para o mesmo retorna **`true`**, caso contrário retornará **`false`**.

Sintaxe: **`array.some(callBack(element, index, array), thisArg)`**

> IMPORTANTE! - O método **`some()`** não modifica o array original.

- **callback**: Função que será executada para cada elemento do array.
  - **`element`**: O elemento atual.
  - **`index`** _(opcional)_: O índice do elemento atual.
  - **`array`** _(opcional)_: O array completo.
- **`thisArg`** _(opcional)_: Um valor para ser usado como `this` dentro da função callback.

```js
let returnAPI = [
  { id: 0, name: "Alex", email: "alex@gmail.com", gender: "M" },
  { id: 1, name: "Maria", email: "maria@ig.com" },
  { id: 2, name: "Joaquim", email: "joaquim@email.net", gender: "M" },
  { id: 3, name: "Eliane", email: "eliane@user.org", gender: "F" },
  { id: 4, name: "Irmã", email: "irma@irma.net", gender: "F" },
];

// every(), verificando se todo array atende a condição de "TER" uma propriedade "gender", retornando true para positivo.
let resultSome = returnAPI.some((value) => {
  // return ! "gender" in value; // condição para testar propriedade (!negação)
  return !value.hasOwnProperty("gender"); // (! negação)
});

console.log(resultSome);
// true
```
