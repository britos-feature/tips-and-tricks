O `echo` é um dos comandos mais simples e usados no **Linux** (e em shells como _bash_, _zsh_, etc.). Ele serve basicamente para **exibir texto na saída echopadrão** (normalmente o terminal).

### ***`echo` interno e externo***

O `echo` geralmente é um **builtin do shell** (`bash`, `zsh`), mas também existe `/bin/echo`.
Algumas opções podem variar entre implementações, por isso em scripts mais portáveis às vezes se recomenda usar `printf`.

***Em resumo:***
- Use **`echo`** para coisas rápidas no terminal.
- Use **`printf`** em **scripts sérios** (principalmente quando precisa de formatação garantida e portabilidade).
<br>
## `echo`

**Sintaxe básica comando `echo`**
	`echo [options] [string]`

### ***Principais usos para o comando `echo`***

- **Exibir mensagens**
	`echo "Hello World!"`

- **Exibir variáveis**
	`echo $HOME`

- **Executando comandos** (substituição de comando) ( ... )
		`echo $(cat arq.txt)`, `echo $(date)` `echo $(ls -l)`

	> **Obs.** a utilização via _substituição de comando_ **não tem quebra de linhas** e obedece os espaços e tabulação inseridos. Para que acha uma formatação correta e necessário que utilize o comando seja executado entre ( **" "** ), exemplo :  `echo "$(ls -l)"` .
<br>
- **Utilizando escapes de caracteres**
	`echo -e Line1\nLine2\t(Tab)`
		saída **→** Line1
			   Line2
				   (Tab)
<br>
- **Sem quebra de linha**
	`echo -n "Enter name: "`
		**→** (o cursor fica na mesma linha da mensagem)
<br>
- **Redirecionamento**
	`echo "Sucess OK" > log.txt`
	`echo "New line" >> log.txt`
<br>
## `printf`

O comando **`printf`** imprime texto formatado, e é inspirado na função `printf()` da linguagem C.
Diferente do `echo`, **não adiciona quebra de linha automaticamente**.

É por **padrão POSIX**, ou seja, funciona de forma consistente em diferentes sistemas (melhor para scripts portáveis).

**Sintaxe básica comando `printf`**
	`printf [format] [arguments]`
		**→**`"formato"` → uma string que contém texto e especificadores de formato.
		**→**`[argumentos...]` → valores que vão substituir os especificadores.
	
### ***Especificadores de format***

**" %s "** **→** String de texto
	`printf "%s\n" "Linux"`  **→** return: <span style="color: blue"><b>Linux</b></span>
<br>
**" %u "** **→** Numero Inteiro 
	`printf "%u\n" 42`  **→** return: <span style="color: blue"><b>42</b></span>
<br>
**" %d "** **→** Inteiro decimal
	`printf "%d\n" -42`  **→** return: <span style="color: blue"><b>-42</b></span>
<br>
**" %f "** **→** Numero real (float)
	`printf "%f\n" 3.14321`  **→** return: <span style="color: blue"><b>3.14321</b></span>
	`printf "%.2f\n" 3.14321`  **→** return: <span style="color: blue"><b>3.14</b></span>
<br>
**" %x "** **→** Inteiro Hexadecimal
	`printf "%x\n" 255`  **→** return: <span style="color: blue"><b>ff</b></span>
	`printf "%X\n" 255`  **→** return: <span style="color: blue"><b>FF</b></span>
<br>
**" %o "** **→** Inteiro em Octal
	`printf "%o\n" 8`  **→** return: <span style="color: blue"><b>10</b></span>
<br>
**" %\% "** **→** Imprime o próprio % 
	`printf "100%% concluido\n`  **→** return: <span style="color: blue"><b>100% concluido</b></span>


### ***Usos principais para o comando `printf`***

- **Definir largura mínima**:
	`printf "%10s\n" "Linux"` **→** return:  <span style="color: blue"><b>Linux (ocupou 10 espaços, alinhado à direita).</b></span>
	<br>
- **Alinhado à esquerda**:
	`printf "%-10s\n "Linux"` **→** return:  <span style="color: blue"><b>Linux (ocupou 10 espaços, alinhado à esquerda).</b></span>
<br>
- **Zeros à esquerda (números)**:
	`printf "%05d\n" 42` **→** return:  <span style="color: blue"><b>00042</b></span>
<br>
- **Exemplo de tabulação
```sh

printf "%-10s %-10s\n" "Name" "Age"
printf "%-10s %-10d\n" "Britos" 50
printf "%-10s %-10d\n" "Maria" 8

```

```sh

# → return
Name      Britos
Maria     8

```
<br>
**Resumo `printf`:

- %s  → string
- %c  → caractere
- %d, %i → inteiro decimal (com sinal)
- %u  → inteiro decimal (sem sinal)
- %o  → octal
- %x, %X → hexadecimal
- %f  → float decimal
- %e, %E → float em notação científica
- %g, %G → float “inteligente” (compacto)
- \%%  → imprime o símbolo %
<br>
- `printf "string \n"` **→** "string"  or `printf "%s\n" "Linux"`  **→** "Linux" (string)
- `printf "%d\n" -42` **→** -42 (numero inteiro com sinal)
- `printf "%u\n" 42` **→** 42 (numero inteiro sem sinal)
- `printf "%x\n" 255`  **→** ff (numeração hexadecimal)
- `printf "%X\n" 255` **→** FF (numeração hexadecimal)
- `printf "%f\n" 3.14159` **→** 3.141590 (float)
- `printf "%.2f\n" 3.14159` **→** 3.14 (float, definição de casas)
- `printf "%e\n" 12345` **→** 1.234500e+04 ()
- `printf "%E\n" 12345` **→** 1.234500E+04
- `printf "%g\n" 12345` **→** 12345 (float inteligente)
- `printf "%G\n" 0.000123` **→** 0.000123 (float inteligente)
- `printf "100%%\n"` **→** 100% (escape imprime sinal %)

- `printf "%10s\n" "Linux"`**→** Linux (alinhamento space 10 right (direita))
- `printf "%-10s\n" "Linux"` **→** Linux (alinhamento space 10 left (esquerda))
- `printf "%05d\n" 42` **→** 00042 (adicionando casa com 0 a esquerda)
- `printf "%+d\n" 42` **→** +42 (numero inteiros com sinais)
- `printf "% d\n" 42` **→** 42 (numero inteiro sem sinais)
