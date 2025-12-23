# Tudo sobre String

### String

Em JavaScript, uma **string** é uma sequência de caracteres usada para representar texto. Ela pode ser definida usando aspas simples (`'`), aspas duplas (`"`) ou crases (`` ` ``) para **template literals**.

**Sintaxe:**

```js
let myString = "Essa é uma string";
```

#### Métodos Comuns para Manipular Strings

1. **`length`** – Obtém o tamanho da string:

```js
let myString = "Essa é uma string";

myString.length;
// return 17
```

2. **`toUpperCase()` e `toLowerCase()`** – Transforma a string em maiúsculas ou minúsculas:

```js
let myString = "Essa é uma string";

myString.toUpperCase();
// return ESSA É UMA STRING

myString.toLowerCase();
//return essa á uma string
```

3. **`trim()`, `trimStart()`, `trimEnd()`** - O método **_`.trim()`_** em JavaScript remove **espaços em branco** do início e do final de uma string, sem modificar o conteúdo dentro da string.

```js
let myString = "     Essa é uma string            ";

// inícia e fim
myString.trim();
// return Essa é uma string

// início
myString.trimStart();
// return "Essa é uma string            "

// fim
myString.trimEnd();
// return "     Essa é uma string"
```

4. **`slice(start, end)`** – Retorna parte da string:

```js
let myString = "Essa é uma string";

myString.slice(0, 4);
// return Essa

myString.slice(4);
// return é uma string

myString.slice(-4);
// return ring

// substring() => Ésse método é igual o método slice(), porém não aceita valores negativos.
myString.substring(0, 4);
// return Essa

myString.substring(4);
// return é uma string

myString.substring(-4);
// return Essa é uma string (não funciona)
```

5. **`replace(old, new)`** – Substitui parte da string:

```js
let myString = "Essa é uma string";

// substituir de um trecho específico (apenas a primeira sequência)
myString.replace("string", "cadeia de caracteres");
// return Essa é uma cadeia de caracters

// string => palavras a ser substituida
// cadeia de caracteres => palavras que substituira 'string'

// (Regex) para substituir todas as ocorrências
myString.replace(/s/g, "S");
// return ESSa é uma cadeia de caracterS

// (Regex) para substituir todas as ocorrências(ignorando maiúsculas/minúsculas)
myString.replace(/e/gi, "S");
// return SSSa S uma cadSia de caractSrS

// Usando uma função no `replace() => Podemos passar uma função para manipular o valor substituído.
let price = "O preço é R$ 100.";
let newPrice = price.replace(/\d+/, (value) => `R$${Number(value) + 50}`);
// return newPrice = 150.
```

6. **`split(separador)`** – Divide a string em pedaços, retornando um ARRAY com os pedaços separado pelo limitador informardo (space).

```js
let myString = "Essa é uma string";
let myString2 = "JavaScript";

myString.split(" "); // space
// return [ 'Essa', 'é', 'uma', 'string' ]

// Usando um limite para dividir a string, o segundo argumento passado define quantas partes a string será dividida.
myString.split(" ", 2);
// return [ 'Essa', 'é']

// Dividir uma string em caracteres individuais (caracteres)
myString2.split("");
// return ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]

// Usar Expressões Regulares para dividir
let dados = "nome:João;idade:25|cidade:São Paulo";
let info = dados.split(/[:;|]/);

console.log(info);
// return ["nome", "João", "idade", "25", "cidade", "São Paulo"]
```

7. **`typeof variable`** - Verificação do tipo de valor anexado a variável.

```js
let myString = "Essa é uma string";

console.log(typeof myString);
// return string
```

8. **`charAt(3)`** - Esse método retorna o caractere de uma string em um índice específico. `index`: Posição do caractere que queremos obter (começa do **0**).

```js
let myString = "Essa é uma string";

// primeiro caracter
myString.charAt(0);
// return E


// ultimo caracter
myString.charAt(myString.length - 1));
// return g
```

9. **`indexOf()`** - retorna o index da primeira posição do charater informado.
   **`index`:** Posição do caractere que queremos obter (começa do **0**).

> **_`indexOf()`_**, pode receber 02 parâmetros (value, index)
> value = caracter a ser procurado
> index = começe a procurar a partir da posição informada

```js
let myString = "Essa é uma string";

myString.indexOf("a"); // com 01 parâmetro -> retorna o index da primeira posição do charater informado.

myString.indexOf("a", 4); // com 02 parâmetro -> retorna o index da primeira posição pesquisando a partir do 02 parâmetro informado.
```

10. **`lastIndexOf()`** - retorna o index da ultima posição do caracter informado.
    **`index`:** Posição da caracter que queremos obter (começa do **0**).

> **_`lastIndexOf()`_**, pode receber 02 parâmetros (value, index)
> value = caracter a ser procurado
> index = começe a procurar a partir da posição informada

```js
let myString = "Essa é uma string";

myString.lastIndexOf("a"); // retorna o index da ultima posição do charater informado.

myString.lastIndexOf("a", 8); // retorna o index da ultima posição do charater informado, terminando a pesquisa no index informado.
```

11. **`search()`** - o método **_`.search()`_** é usado para encontrar o índice da primeira correspondência de uma expressão regular em uma string. Ele retorna o índice da primeira ocorrência ou `-1` se nenhuma correspondência for encontrada. procura pelo valor informado na string, retornado o index de inicio do valor encontrado

```js
let myString = "Essa é uma string";

myString.indexOf("uma")); // 7
myString.search("uma")); // 7
myString.search(/u.a/)); // 7 (regex suportado)

// Diferença entre `.search()` e `.indexOf()`
// `.search()` funciona apenas com expressões regulares.
// `.indexOf()` funciona com strings normais e **não** suporta regex.
```

12. **`match()`** - o método `.match()` é usado para encontrar todas as correspondências de uma expressão regular dentro de uma string. Ele retorna um array com as correspondências encontradas ou `null` se nenhuma for encontrada.

```js
let myString = "Essa é uma string";

// procura pelo valor informado na string, retornado UM ARRAY com descrição e o primeiro valor e encontrados na string.
myString.match(/string/);
// return [ 'string', index: 11, input: 'Essa é uma string', groups: undefined ]

// procura pelo valor informado na string, retornando um array com todos valores encontrados na string.
myString.match(/string/gi);
//return [ "string" ]
```

#### Diferença entre `.match()` e `.search()`

| Método      | Retorno                            | Aceita `/g`? | Retorna índice? |
| ----------- | ---------------------------------- | ------------ | --------------- |
| `.search()` | Índice da primeira correspondência | ❌ Não       | ✅ Sim          |
| `.match()`  | Array com correspondências         | ✅ Sim       | ❌ Não          |

13. **`padStart()`** - preenche uma string com caracteres no **início** até atingir um determinado comprimento.

```js
let myString = "Essa é uma string";

// preencher o inicio da string com caracter informado até atingir a quantidade informada = (qtd + 'character') - QTD acima do valor de index da STRING
myString.padStart(20, "*");
// return *************Essa é uma string

// preencher o fim da string com caracter informado até atingir a quantidade informada = (qtd + 'character') - QTD acima do valor de index da STRING
myString.padEnd(20, "*");
// return Essa é uma string*************

// exemplo utíl
let varNUMBER = "16516516565";

varNUMBER.slice(0, 8).padStart(11, "*"); // retorna variavel preenchida dessa forma: ********565.

varNUMBER.slice(0, 8).padEnd(11, "*"); // retorna variavel preenchida dessa forma: 165********.
```

14. **`endsWith()`** - método que verifica se a string termina com um determinado sufixo. Opcionalmente, você pode fornecer um comprimento para considerar apenas uma parte da string.

```js
const texto = "hello world";

console.log(texto.endsWith("world")); // true
console.log(texto.endsWith("hello")); // false
console.log(texto.endsWith("lo", 5)); // true (considera apenas os primeiros 5 caracteres: "hello")
```

15. **`repeat()`** - o método **`repeat()`** permite repetir uma string um número específico de vezes.

```js
console.log("ha ".repeat(5)); // return "ha ha ha ha ha ha"
```

16. **`includes()`** - o método `includes()` verifica se uma string contém um determinado trecho de texto.

```js
let str = "Apreendendo JavaScript na integra!";

console.log(str.includes("JavaScript")); // return true
```
