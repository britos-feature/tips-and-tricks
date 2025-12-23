
## _Command **`while`**_

O comando **`while`** no **shell Unix** é uma estrutura de repetição (loop) que executa um bloco de comandos enquanto uma condição for verdadeira.

***Sintaxes básic***

```sh

while [ condittion ]
do
	commands
done

```
<br>
### _Funcionamento passo a passo_

1. O shell **avalia a condição** (normalmente um comando ou expressão dentro de colchetes `[ ]`).
2. Se a condição retornar **status 0** (verdadeiro, em shell isso significa “sem erro”), o bloco entre `do` e `done` é executado.
3. Depois de executar o bloco, a condição é testada novamente.
4. O processo se repete até a condição retornar **status diferente de 0** (falso, ou seja, erro).
5. Quando a condição for falsa, o loop termina e a execução continua após o `done`.


***Exemplo, contagem simples***

```sh

contador=1

while [ $contador -le 5 ]
do
    echo "Número: $contador"
    contador=$((contador + 1))
    sleep 1
done

```


***saída***

```sh

numero --> 1
numero --> 2
numero --> 3
numero --> 4
numero --> 5

```
<br>
