
REGEX, significa **_Regular Expression_** (expressão regular).
Uma **linguagem de padrões** usada para buscar, filtrar, validar ou manipular textos.  

Um **REGEX** é composto de **caracteres literais** (aquilo que você quer encontrar exatamente) e **metacaracteres** (símbolos especiais que dão regras de busca).

***Contents***:

- Metacaracteres Principais (básicos)
	- [[#^e93223| Âncoras]],
	- [[#^fc1123|Quantificador]]
	- [[#^094f56|Class de caractere]]
	- [[#^ab8090|Agrupador]]
	- [[#^757fb2|Atalhos pré definidos]]
<br>

- Metacaracteres Avançado.
	- [[#^a8100b|Greedy and Lazy]]
	- [[#^5fb484|Lookahead and Lookbehind]]

<br>
## ***Metacaracteres Principais (básicos)***


1. ***ÂNCORAS (posições no texto)***

	**" ^ "** →  _**circunflexo,** corresponde ao inicio da string._
		regex: <span style="color:blue"><code><i>^Inicio</i></code></span> 
			→ _match **Inicio** se estiver no começo da string_ ^e93223

---
	**" $ "** → **_cifrão,** corresponde ao fim da string_
		regex: <span style="color:blue"><code><i>Fim$</i></code></span> 
			→ _match **Fim** se estiver no final da string_

---
	**" . "** → **_ponto,** corresponde a qualquer caractere, menos espaço \s e \t\r
		regex: <span style="color:blue"><code><i>Fim$</i></code></span> 
			→ _match **Fim** se estiver no final da string_

---
---


2. ***QUANTIFICADORES (quantidade de repetição)***

	**" \* "** → **_asterisco,** corresponde a **0** ou **mais vezes_** <span style="color:red"><i>(opcional)</i></span>
		regex: <span style="color:blue"><code><i>a*</i></code></span> 
			→ _match <span style="color:red"><b>" "</b></span>**, " a ", " aa ", " aaa ", ...**  ^fc1123

---
	**" + "** → **_adição,** corresponde a **1** ou **mais vezes_** <span style="color:red"><i>(obrigatório)</i></span>
		regex: <span style="color:blue"><code><i>a+</i></code></span> 
			→ _match <span style="color:red"><b>" a "</b></span>**, " aa ", " aaa ", ...**

---
	**" ? "** → **_interrogação,** corresponde a **0** ou **uma vez_** <span style="color:red"><i>(opcional)</i></span>
		regex: <span style="color:blue"><code><i>a?</i></code></span> 
			→ _match <span style="color:red"><b>" ", " a "</b></span>

---
	**" {n} "** → **_chaves,**  corresponde exatamente **" n " vezes_**
		regex: <span style="color:blue"><code><i>a{2}</i></code></span> 
			→ _match <b><i>" aa "</i></b>

---
	**" {n,} "** → _pelo menos  **" n " vezes_**
		regex: <span style="color:blue"><i>a{2,}</i></span>
			→ _match <b><i>" aa ", "aaa", "..."</i></b>

----
	**" {n, m} "** → _entre **" n "**  e **m** vezes_
		regex: <span style="color:blue"><i>a{2,4}</i></span>
			→ _match <b><i>" aa ", "aaa", "aaaa"</i></b>

---
	**" {,n} "** → **_0** ou **" n " vezes_**
		regex: <span style="color:blue"><i>a{,2}</i></span>
			→ _match <b><i>" ", " a ", " aa ", "aaa", "..."</i></b>

---
---


3.  ***CLASSES DE CARACTERES

	**" \[ abc \] "** → **_colchetes,** corresponde a qualquer um dos caracteres dentro dos  **colchetes_**  <span style="color:red"><i>(ou)</i></span>
		regex:  <span style="color:blue"><i>[ abc ]</i></span>
			→ _match <b><i>" a", " b" ou " c"</i></b> ^094f56

---
	**" \[ a-c \] "** → **_colchetes,** corresponde a **um range** de caracteres minúsculos_  
	**" \[ A-C \] "** → **_colchetes,** corresponde a **um range** de caracteres maiúsculos_  
	**" \[ 0-9 \] "** → **_colchetes,** corresponde a **um range** de dígitos_  
		regex:  <span style="color:blue"><i>[ a-c ]</i></span>
		regex:  <span style="color:blue"><i>[ A-C ]</i></span>
		regex:  <span style="color:blue"><i>[ 0-9 ]</i></span>
			→ _match <b><i>" a ", " b " e " c "</i></b>
			→ _match <b><i>" A ", " B " e " C "</i></b>
			→ _match <b><i>" 0 ", " 1 ", " 2 ", " 3 ", " 4 ", " 5 ", " 6 ", " 7 ", " 8 " e " 9 "</i></b>

---
	**" \[ ^abc \] "** → **_colchetes,** corresponde a qualquer exceto caracteres dentro dos **colchetes_**  <span style="color:red"><i>(ou)</i></span> 
		regex:  <span style="color:blue"><i>[ ^abc ]</i></span>
			→ _match <b><i>" d ", " e ", " f ", " ... "</i></b>

---
4. ***AGRUPADOR***

	**" ( abc ) "** → **_parenteses,** corresponde a um **agrupamento de caracteres**, como um unidade_  
		regex: <span style="color:blue"><i>( abc )</i></span>
			→ _match_ <b><i>" abc "</i></b> ^ab8090

---
---


4. ***ATALHOS PREDEFINIDOS (shorthands)***

	**" \d e \D "**  ^757fb2
	- <span style="color:red"><b>\d</b></span> → *corresponde a **dígitos numéricos** (equivalente a \[ 0-9 \] )*
	- <span style="color:red"><b>\D</b></span> → *corresponde a **a não dígitos numéricos** (tudo que não for de 0 - 9 )*
		regex: <span style="color:blue"><i>\d1234</i></span>
			→ _match_ <b><i>" 1234 "</i></b>

---
	**" \w e \W "** 
	- <span style="color:red"><b>\w</b></span> → *corresponde a **caractere de palavras,** letras, números e  _underline 
	(equivalente a \[a-zA-Z_\]*
	- <span style="color:red"><b>\W</b></span> → *corresponde a **a não caractere de palavras** (tudo fora do conjunto **\w**, inclui também caractere com acentos )*
		regex: <span style="color:blue"><i>\w+</i></span>
			→ _match_ <b><i>" abc123_ "</i></b>

---
	**" \s e \S "** 
	- <span style="color:red"><b>\s</b></span> → *corresponde a **espaço em branco** (espaço, tabulação, quebra de linha, etc.).
	- <span style="color:red"><b>\S</b></span> → *corresponde a **a não espaço em branco**.*
		regex: <span style="color:blue"><i>\s+</i></span>
				→ _match_ <b><i>"      "</i></b>  > ***encontra todos os espaços múltiplos.***
				→ _match_ <b><i>"      "</i></b>  > ***encontra palavras contínuas ignorando espaços.***

---
	**" \b e \B "** 
	- <span style="color:red"><b>\b</b></span> → *corresponde a **boundary** (fronteira de palavra).*
	- É o limite entre um `\w`caractere de palavras, ou um `\W`.
	- <span style="color:red"><b>\B</b></span> → *corresponde a **não boundary** (lugares que **não** são fronteira).
		regex: <span style="color:blue"><i>\b1234\b</i></span>
			→ _match_ <b><i>" 1234 "</i></b>
		regex: <span style="color:blue"><i>\B34</i></span>
			→ _match_ <b><i>" 34 "</i></b>

---
	**Escapes especiais**
	- `\n` → quebra de linha
	- `\r` → retorno de carro (Windows: `\r\n`)
	- `\t` → tabulação
	- `\0` → byte nulo
	- `\f` → form feed (raro)

<br>
***TABELS RESUMIDA DE METACARACTERE BÁSIO***

| Metacaractere | Significado / Função                    | Exemplo          | Resultado / Explicação                          |
| ------------- | --------------------------------------- | ---------------- | ----------------------------------------------- |
| `.`           | Qualquer caractere (exceto nova linha)  | `a.b`            | Casa `acb`, `a1b`, `a-b`                        |
| `[]`          | Conjunto de caracteres                  | `[abc]`          | Casa `a`, `b` ou `c`                            |
| `[^]`         | Negação dentro de colchetes             | `[^0-9]`         | Casa qualquer caractere que **não** seja dígito |
| `\`           | Escape ou atalhos                       | `\.`             | Casa literalmente um ponto `.`                  |
| `\d`          | Dígito `[0-9]`                          | `\d+`            | Casa `123`, `4567`                              |
| `\D`          | Não dígito                              | `\D+`            | Casa `abc`, espaços, símbolos                   |
| `\w`          | Palavra (letra, número, underscore)     | `\w+`            | Casa `user_123`                                 |
| `\W`          | Não palavra                             | `\W+`            | Casa espaços, pontuação                         |
| `\s`          | Espaço em branco (tab, espaço, newline) | `\s+`            | Casa espaços múltiplos                          |
| `\S`          | Não espaço                              | `\S+`            | Casa palavras contínuas                         |
| `^`           | Início da linha                         | `^Oi`            | Casa `Oi` só no começo                          |
| `$`           | Fim da linha                            | `fim$`           | Casa `fim` só no final                          |
| `()`          | Grupo (captura)                         | `(ab)+`          | Casa `ab`, `abab`, `ababab`                     |
| `{n}`         | Quantidade exata                        | `\d{3}`          | Casa exatamente 3 dígitos                       |
| `{n,}`        | Pelo menos n vezes                      | `a{2,}`          | Casa `aa`, `aaa`, `aaaa`                        |
| `{n,m}`       | Entre n e m vezes                       | `\d{2,4}`        | Casa `12`, `123`, `1234`                        |
| `*`           | Zero ou mais vezes                      | `ba*`            | Casa `b`, `ba`, `baa`, `baaa`                   |
| `+`           | Uma ou mais vezes                       | `ba+`            | Casa `ba`, `baa`, `baaa` (não casa `b`)         |
| `?`           | Zero ou uma vez / lazy                  | `colou?r`        | Casa `color` e `colour`                         |
| \|            | \| (Ou) um / outro                      | <code>a\|b<code> | Casa `a`, `b`                                   |
| `\b`          | Limite de palavra                       | `\bcar\b`        | Casa `car` isolado, mas não em `cargo`          |
| `\B`          | Não limite de palavra                   | `\Bcar\B`        | Casa `car` dentro de uma palavra, como `scary`  |
<br>

---
---

## ***Metacaracteres Avançados***

1. ***GREEDY and LAZY (incremento para quantificadores)***
	Em  regex, um **quantificadores** define **quantas vezes um padrão pode se repetir**.

	- ***Greedy "guloso"*** (comportamento default), quantificador que capturar **o máximo possível** de caracteres enquanto ainda consegue casar com a expressão.
		- <span style="color:blue"><b><i>Greedy</i> =</b> <u>"quero tudo o que eu puder pegar"</u></span>

	- ***Lazy "preguiçoso"***, é a montagem de um quantificador que capturar **o mínimo possível** de caracteres enquanto ainda consegue casar com a expressão.
		- <span style="color:blue"><b><i>Lazy</i> =</b> <u>"quero só o necessário"</u></span>

	<b><i>Sintaxes:</i></b>
	 ^a8100b
```js
// string 
const html = "<p> Hello World! </p> <p> Welcome... </p>";

// Comportamento default ( Greedy )
var expReg = /<.+>.+<\/.+>/g;
// match = [ "<p> Hello World! </p> <p> Welcome... </p>" ]

// Comportamento ( Lazy )
var expReg = /<.+?>.+?<\/.+?>/g;
// match = [ "<p> Hello World! </p>", "<p> Welcome... </p>" ]


// O `?` **após um quantificador** transforma ele de greedy para lazy.
// Sem o `?`, ele é greedy por padrão.
 
```
<br>

---

2. ***LOOKAHEAD E LOOKBEHIND*** (não consomem texto) 

	Os **lookarounds** termo mais conhecidos, servem para "olhar ao redor" de um padrão, mas **sem consumir os caracteres olhados**. Ou seja, eles testam o que vem antes ou depois do ponto atual, mas não fazem parte do resultado da captura.
	<br> ^5fb484
	-  ***Positive Lookahead*** <i><span style="color:blue"><b>( ?=abc )</b></span></i>  → _“só casa se **à FRENTE houver** o padrão especificado”_.
	
```js
// ****** Positive Lookahead ******* //

// string
const str = "Hello World, welcome!";

// Positive Lookahead (só casa se a frente houver o padrão especificado).
const regExp = /.+(?=welcome.)/g; // (?=...)

// Consumindo regex
str.math(regExp); // return [ "Hello World, "]
// Ñ inclui no return o padrão especificado do Lookhead (?=...)

```
<br>
		-  ***Negative Lookahead*** <i><span style="color:blue"><b>( ?!abc )</b></span></i>  → _“só casa se **à FRENTE não houver** o padrão especificado”_.

```js
// ****** Negative Lookahead ******* //

// string
const str = "Hello World, welcome!";

// Negative Lookahead (só casa se a frente ñ houver o padrão especificado).
const regExp = /Hello World,(?! other)/g; // (?!...)

// Consumindo regex
str.match(regExp); // return [ "Hello World," ]
// return NULL, caso o padrão especificado existir na regex.
 
```
<br>
		-  ***Positive Lookbehind*** <i><span style="color:blue"><b>(?&lt;=...)</b></span></i>  → _“só casa se **ATRÁS houver** o padrão especificado”_.

```js
// ****** Positive Lookbehind ****** //

// string
const str = "Hello World, welcome!"

// Positive Lookbehind (só casa se ATRÁS houver o padrão especificado)
const regExp = /(?<=Hello).+/g; // return [" World, welcome!"]
// Ñ inclui no return o padrão especificado do Lookbehind (?<=...)

```
<br>
		-  ***Negative Lookbehind*** <i><span style="color:blue"><b>(?&lt;!...)</b></span></i>  → _“só casa se **ñ for PRECEDIDO** pelo padrão especificado”_.

```js
// ****** Negative Lookbehind ****** //

// string
const str = "Hello World, welcome!"

// Negative Lookbehind (só casa se não for precendido pelo padrão especificado)
const regExp = /(?<!Hello )Word, welcome!/g; // return [ "World, welcome!" ]
// return NULL, caso padrão especificado seja encontrado no Lookbehind(?<!...)              
```
<br>

***Resumo:***

Existem **quatro** tipos principais **Lookarounds**:

| Tipo       | Sintaxe             | Nome                                              |
| ---------- | ------------------- | ------------------------------------------------- |
| `(?=...)`  | Lookahead positivo  | Verifica se **à frente** existe o padrão.         |
| `(?!...)`  | Lookahead negativo  | Verifica se **à frente** **não** existe o padrão. |
| `(?<=...)` | Lookbehind positivo | Verifica se **atrás** existe o padrão.            |
| `(?<!...)` | Lookbehind negativo | Verifica se **atrás** **não** existe o padrão.    |

---

3. ***GROUP CAPTURE (para uso posteriormente)***

***Group capture*** é quando colocado um padrão entre parênteses `(...)`, o regex **captura** o texto correspondente e armazena em um **grupo numerado** ***($1, $2, $3, $4, ...).***

***Funcionamento:***
- primeiro armazena
- depois utiliza *(normalmente utilizado com **replace()**)*

***Exemplo:***

```js
// ****** Group Capture ****** //

// String
const date = "09-10-2025";  // date format USA MM/dd/AAAA

const regExp = /(\d{2}).(\d{2}).(\d{4})/;
/*
group 1 = (\d{2}) corresponde a " $1 "
group 2 = (\d{2}) corresponde a " $2 "
group 3 = (\d{4}) corresponde a " $3 "
*/

// Group Capture - "alterando ordem return para "date format" PT_br dd/mm/aaaa"
str.repalce(regExp, "$2/$1/$3"); // return PT-br date dd/mm/aaaa"

```
<br>

---

4. ***Backreference (para uso dentro da própria regex or em uma substituição)***

	Um **backreference** é quando você usa o conteúdo de um grupo **dentro do próprio regex** (ou em uma substituição).  Ele garante que um trecho se repita exatamente.

***Exemplo prático:  "detectar palavras repetidas em uma string."

```js
// ****** Backreference (retrovisor)****** //

/*
group 1 corresponde a backreference " \1 "
group 2 corresponde a backreference " \2 "
group 3 corresponde a backreference " \3 "
...
*/

// String
const str = "Hello Hello World, welcome!";

const regExp = /(\w+)\s+\1/g; // analizando "works" repetidas em sequencia.
// capturando grupo1 (\w+) e utilizando backreference " \1 " na mesma regex.

str.match(regExp); // return [ "Hello", "Hello" ]

```
<br>
***Exemplo prático:  "Validar tags html/XML balanceadas".***

```js
// ****** Backreference (retrovisor)****** //

/*
group 1 corresponde a backreference " \1 "
group 2 corresponde a backreference " \2 "
group 3 corresponde a backreference " \3 "
...
*/

// html
const html = "<p>Hello World, welcome!</p>;

const regExp = /<(\w+)>.*?<\/\1>/g; // analizando abertura e fechamento de tags corretos.
// capturando grupo1 (\w+) e utilizando backreference " \1 " na mesma regex.

regExp.test(html); // return "true"
// abertura e fechamento da tag ok!

```
<br>
***Cheatsheet de Backreferences***

| Caso de uso                                             | Regex                                                   | Exemplo de entrada             | Saída / Match         |
| ------------------------------------------------------- | ------------------------------------------------------- | ------------------------------ | --------------------- |
| **1. Palavra repetida** (detectar duplicação)           | `\b(\w+)\s+\1\b`                                        | `A aula de regex regex é útil` | Match: `regex regex`  |
| **2. Tags HTML balanceadas**                            | `<(\w+)>.*?</\1>`                                       | `<b>texto</b>`                 | Match: `<b>texto</b>` |
|                                                         |                                                         | `<b>texto</i>`                 | ❌ Não casa            |
| **3. Palíndromo curto**                                 | `(\w)(\w)\2\1`                                          | `abba`                         | Match: `abba`         |
| **4. Data com separador consistente**                   | `(\d{2})([-/])(\d{2})\2(\d{4})`                         | `10/09/2025`                   | Match válido          |
|                                                         |                                                         | `10-09-2025`                   | Match válido          |
|                                                         |                                                         | `10/09-2025`                   | ❌ Inválido            |
| **5. Três caracteres repetidos** (senhas fracas, erros) | `(.)\1\1`                                               | `abc111xyz`                    | Match: `111`          |
| **6. Remover duplicados (JS)**                          | `/\b(\w+)(\s+\1)+\b/g` → `"$1"`                         | `"palavra palavra palavra"`    | `"palavra"`           |
| **7. Reutilizar parte do texto**                        | `(\d{2})/(\d{2})/(\d{4})` → Substituir por `"$3-$2-$1"` | `10/09/2025`                   | `2025-09-10`          |
