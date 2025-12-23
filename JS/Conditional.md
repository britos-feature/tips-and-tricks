
# Avalições e condicional

### Short-circuit evalutio (avalições Curto Circuito)
Short-circuit são avaliação que avaliam **valores JS**,  se os mesmo são verdadeiros(true) ou falsos(false), ***retornando o próprio valor literal***.

**Resumo:**
**short-circuit evaluation** (avaliação de curto-circuito) é um comportamento lógico onde uma expressão é avaliada apenas até o ponto necessário para determinar seu resultado final. Isso acontece em operadores lógicos como **`&&` (AND)** e **`||` (OR)**.

#### Vantagem ao utiliza-se de "short-circuit"
 - **Melhora a Performance ->** evita avaliações desnecessárias ao interromper a execução assim que o resultado da expressão é conhecido. Isso economiza tempo e recursos.
 <br>
 
 - **Simplicidade no Código ->** escrever estruturas `if` desnecessárias, você pode usar `&&` ou `||` para definir valores de fallback ou executar ações.
 <br>
 
 - **Substitui  " if " em Algumas Situações ->** Em alguns casos, `&&` pode ser usado para executar uma ação apenas se uma condição for verdadeira.
 <br>
 
 - **Ajuda a Evitar Erros (`Null` ou `Undefined`) ->** Você pode usar `||` para garantir que uma variável tenha um valor válido antes de usá-la.

> *Valores literais que podem ser compardos como **false**
> false, '' "", NaN, null, undefined e 0


Exemplos:

1. Se o primeiro operando de `&&` for `false`, o segundo nem precisa ser avaliado.

```js
function verificar() {
  console.log("Função chamada!");
  return true;
}

console.log(false && verificar()); // false (a função não é chamada)
console.log(true || verificar());  // true (a função não é chamada)
```


2. Executar uma função apenas se a variável for verdadeira:

```js
	let autenticado = true;
autenticado && console.log("Usuário logado!"); // Exibe a mensagem
```


3. Definir um valor padrão sem `if`

```js
let usuario = "";
let nome = usuario || "Visitante"; // Se `usuario` for vazio, assume "Visitante"
console.log(nome); // "Visitante"
```

4. Garantir que uma variável tenha um valor válido antes de usá-la utilizando ||

```js
let config = null;
let tema = config?.tema || "light"; // Se `config` for null, usa "light"
console.log(tema); // "light"
```



5. Retornando valor literal (sempre procurando um valor TRUE(verdadeiro)).

```js

const varSHORTCIRCUIT_ = 'a'|| 'b'|| 'c'|| 'd'|| 'e'; 
// return 'a' (primeiro valor TRUE(verdadeiro)).

const lastTRUE = 'a'&& 'b'&& 'c'&& 'd'&& 'e'; 
// return 'e' (ultimo valor TRUE(verdadeiro)).  

const firstFALSE = 0 && false && '' && NaN && null && undefined; 
// return 0 (primeiro valor FALSE(falso)).

const lastFALSE = 0 || false || '' || NaN || null || undefined; 
// return undefined (ultimo valor FALSE(falso)).
```


**Conclusão**
O short-circuit em JavaScript **torna o código mais eficiente, legível e seguro**, evitando avaliações desnecessárias e ajudando a definir valores padrão.
<br>

### Ternary operator (operador ternário)

O **operador ternário** em JavaScript é uma forma concisa de escrever um **if/else** em uma única linha.

#### Funcionamento:

- Se a **condição** for `true`, a expressão antes dos `:` será executada.
- Se a **condição** for `false`, a expressão depois dos `:` será executada.

#### Exemplos:
 ```js
 
// Verificando se um número é par ou ímpar
let numero = 10;
let resultado = (numero % 2 === 0) ? "Par" : "Ímpar";
console.log(resultado); // Saída: "Par"


// Definindo uma mensagem com base na idade
let idade = 18;
let podeDirigir = (idade >= 18) ? "Pode dirigir" : "Não pode dirigir";
console.log(podeDirigir); // Saída: "Pode dirigir"


// Usando operador ternário aninhado
let nota = 7;
let status = (nota >= 7) ? "Aprovado" : (nota >= 5) ? "Recuperação" : "Reprovado";
console.log(status); // Saída: "Aprovado"
```

O operador ternário é útil para expressões simples, mas para lógica mais complexa, um **`if/else`** tradicional pode ser mais legível.


