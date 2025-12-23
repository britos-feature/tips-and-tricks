# Number
Em JavaScript, o tipo **`Number`** é usado para representar valores numéricos, incluindo inteiros e pontos flutuantes. Aqui estão alguns pontos importantes sobre **`Number`:**

#### Criando Números
Você pode declarar números diretamente

```js
let num1 = 10;      // Inteiro
let num2 = 10.5;    // Ponto flutuante
```
	
Ou usando o objeto **`Number`**

```js
let num3 = Number(20);
let num4 = new Number(30); 
// Evite usar "new Number()", pois cria um objeto, não um valor primitivo

console.log(num4.valueOf());
// return 30
// o método valueOf() em JavaScript retorna o valor primitivo de um objeto `Number`.
```


#### Métodos Úteis do `Number`

- **`toFixed(n)`:** Define o número de casas decimais.

```js
let num = 5.6789;
console.log(num.toFixed(2)); // "5.68"
```


- **`toPrecision(n)`:** Define a quantidade total de dígitos.

```js
let num = 123.456;
console.log(num.toPrecision(4)); // "123.5"
```


- **`toString()`:** Converte um número para string.

```js
let num = 255;
console.log(num.toString());   // "255"
console.log(num.toString(16)); // "ff" (conversão para hexadecimal)
```


- **`isNaN(value)`:** Verifica se um valor não é um número.

```js
console.log(isNaN(123));      // false
console.log(isNaN("abc"));    // true
```


- **`isFinite(value)`:** Verifica se o número é finito.

```js
console.log(isFinite(10));      // true
console.log(isFinite(Infinity)); // false
```


- **`isInteger(numberVariable)`:** verificando se o numero é um inteiro, return 'true' or 'false'.

```js
console.log(Number.isInteger(10)); // true
console.log(Number.isInteger(10.5)); // false
```


### Propriedades Importantes do `Number`

- **`Number.MAX_VALUE`:** Maior número possível em JavaScript.
- **`Number.MIN_VALUE`:** Menor número positivo representável.
- **`Number.POSITIVE_INFINITY`:** Representa o infinito positivo.
- **`Number.NEGATIVE_INFINITY`:** Representa o infinito negativo.
- **`Number.NaN`:** Representa "Not-a-Number" (quando uma operação não faz sentido matematicamente).


### Operações Matemáticas Básicas
Em JavaScript, você pode realizar diversas operações matemáticas básicas utilizando operadores aritméticos. Aqui está um resumo das operações mais comuns:

### 1. Operadores Aritméticos

| Operação                  | Operador | Exemplo  | Resultado |
| ------------------------- | -------- | -------- | --------- |
| Adição                    | `+`      | `10 + 5` | `15`      |
| Subtração                 | `-`      | `10 - 5` | `5`       |
| Multiplicação             | `*`      | `10 * 5` | `50`      |
| Divisão                   | `/`      | `10 / 5` | `2`       |
| Módulo (resto da divisão) | `%`      | `10 % 3` | `1`       |
| Exponenciação             | `**`     | `2 ** 3` | `8`       |

#### Exemplos:

```js
let a = 10;
let b = 3;

console.log(a + b);  // 13
console.log(a - b);  // 7
console.log(a * b);  // 30
console.log(a / b);  // 3.3333...
console.log(a % b);  // 1
console.log(a ** b); // 1000 (2³)
```

#### 2. Incremento e Decremento

|Operação|Operador|Exemplo|Resultado|
|---|---|---|---|
|Incremento|`++`|`let x = 5; x++`|`6`|
|Decremento|`--`|`let y = 5; y--`|`4`|

#### Exemplo:

```js
let x = 5;
x++; // Agora x é 6
console.log(x);

let y = 10;
y--; // Agora y é 9
console.log(y);
```


#### Pré-incremento vs. Pós-incremento

```js
let num = 5;

console.log(num++); // 5 (primeiro retorna, depois incrementa)
console.log(num);   // 6

let num2 = 5;
console.log(++num2); // 6 (incrementa antes de retornar)
```


#### 3. Métodos Matemáticos (`Math`)
Além dos operadores básicos, o objeto `Math` fornece métodos úteis para cálculos matemáticos:

**Arredondamento**
- **`Math.round(x)`:** Arredonda para o inteiro mais próximo.
- **`Math.floor(x)`:** Arredonda para baixo.
- **`Math.ceil(x)`:** Arredonda para cima.
- **`Math.trunc(x)`:** Remove a parte decimal.

```js
console.log(Math.round(4.6));  // 5
console.log(Math.floor(4.9));  // 4
console.log(Math.ceil(4.1));   // 5
console.log(Math.trunc(4.9));  // 4
```

**Valor absoluto e sinais**
- **`Math.abs(x)`:** Retorna o valor absoluto.
- **`Math.sign(x)`:** Retorna `1` se positivo, `-1` se negativo, e `0` se zero.

```js
console.log(Math.abs(-10));  // 10
console.log(Math.sign(-5));  // -1
console.log(Math.sign(0));   // 0
console.log(Math.sign(8));   // 1
```


**Potências e Raízes**
- **`Math.pow(x, y)`:** Eleva `x` à potência `y`.
- **`Math.sqrt(x)`:** Retorna a raiz quadrada.
- **`Math.cbrt(x)`:** Retorna a raiz cúbica.

```js
console.log(Math.pow(2, 3));  // 8
console.log(Math.sqrt(16));   // 4
console.log(Math.cbrt(27));   // 3
```


**Mínimo e Máximo**
- **`Math.min(x, y, ...)`:** Retorna o menor número.
- **`Math.max(x, y, ...)`:** Retorna o maior número.

```js
console.log(Math.min(10, 5, 8));  // 5
console.log(Math.max(10, 5, 8));  // 10
```


**Números Aleatórios**
- **`Math.random()`:** Retorna um número aleatório entre 0 e 1.

```js
console.log(Math.random());          // Exemplo: 0.7234
console.log(Math.random() * 10);     // Exemplo: 7.234
console.log(Math.floor(Math.random() * 10) + 1); // Número aleatório entre 1 e 10
```
