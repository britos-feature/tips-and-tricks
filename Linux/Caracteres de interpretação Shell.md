***Caracteres de interpretação do shell*** (também chamados de **metacaracteres** ou **caracteres especiais**).

***Resumindo:*** 
Os **caracteres de interpretação** no shell são responsáveis por **expansão, redirecionamento, controle de fluxo e substituição de comandos**.
<br>
OBS: **Caracteres de interpretação** são diferentes de **Expansão de parâmetros**
#### Mais detalhes sobre o **[[Parameter expansion|Expansão de parâmetros]]**

---

### ***Principais caracteres de interpretação do Shell.***

- " ***space*** e ***tab*** " = _são usados para **separar argumentos** de um comando._

```sh
ls -l /etc # ls = command, -l = options, /etc = arguments (path)
```
<br>
- " **`*` (asterisco)** "  = _**Coringa**: representa zero ou mais caracteres._

```sh
ls *.txt   # lista todos os arquivos terminados em .txt
```
<br>
- " **? (interrogação)** " = _Representa **um único caractere**._

```sh
ls arq?.txt   # casa com arq1.txt, arqa.txt, mas não arq12.txt
```
<br>
- " **\[ ] (colchetes)** " = _Especificam **conjunto ou intervalo** de caracteres._

```sh
ls arq[1-3].txt   # arq1.txt, arq2.txt, arq3.txt
ls arq[a-c].txt   # arqa.txt, arqb.txt, arqc.txt
ls arq[ac].txt    # (ou) arqa.txt, arqc.txt
```
<br>
- " **{ } (chaves)** " = _expansão de lista_.

```sh
echo {a,b,c}.txt   # gera a.txt b.txt c.txt
mkdir folder{1..3} # create folder1, folder2, folder3
```
<br>
- " **' (áspas simples)** " = _Impedem interpretação de metacaracteres (literal)._

```sh
echo '*' # return " * "
```
<br>
- " **" "*** **(áspas duplas)**" = Impedem a maioria das interpretações, **mas ainda permitem variáveis e comandos**.

```sh
echo "$HOME"
```
<br>
- " **\\ (barra invertida)** " = **Escapa** o próximo caractere, fazendo-o ser interpretado como literal.

```sh
echo \* # saída: *
```
<br>
- " **$ (cifrão)** " = Usado para **variáveis** e **substituições**.

```sh
echo $USER
```
<br>
-  " **(crase / backticks)** ou **$( )** " = **Substituição de comando**: executa e retorna o resultado.

```sh
echo My Operation system is `uname` # Só para commandos.
echo "My Operation system is $(uname)" # Só para commandos.
echo "My name is0 $USER" # Isso é para variaveis.
```
<br>
- " **; (ponto e vírgula)**  " = Permite executar vários comandos em sequencia.

```sh
echo oi ; echo tchau
```
<br>
- " **&&, ||** "
	- Execução de condicional:
		- **`&&`** só executa o próximo se o anterior teve sucesso.
		- **`||`** executa o próximo se o anterior falhou.

```sh
mkdir teste && cd teste
```
<br>

---

### ***Decifrando uso das Aspas(" "), dos Apóstrofes/ plics (' ') e Contra barra ( \ ) no shell***

Esse Caracteres no **SHELL** são usados para impedir interpretações indevidas.

-  **Aspas " "**  - no **SHELL**, utilizando-se de as **Aspas " "** ele irá **INTERPRETA** as:  Crases, a Contra barras e o Cifrão.

- **Apóstrofes/ plics ' '** - no **SHELL**, utilizando-se de **Apóstrofes ' '** ele não irá **INTERPRETAR** nada.

- **Contra barra \\** - no **SHELL** a **Contra barra \\** serve para esconder o caractere seguinte.

#### ***Entenda a explicação dos interpretadores:***

```shell
# Atribuição de variável
var=x      x  # false/ no SHELL Atribuição não pode haver espaço ou tabs

# Atribuição de variável
var='x       x' # true

# Exibindo valor da variável atribuida.

echo '$var' # utilização de Apóstrofes não há INTERPLETAÇÂO de caracteres retornando o que foi chamado pelo comando ECHO.
$var

echo $var # retorna o valor da variável apenas com UM ESPAÇO EM BRANCO, eliminando os excedentes, devido uma variável de sistemas $IFS que corresponde a (espaço, tabs, enter)
$x x

echo "$var" # retorna o valor da variável COMO FOI ATRIBUIDO, devido as ASPAS que estão sendo INTERPLETADAS, entendendo que o comando ECHO convocando a variável $var.
$x      x
```
<br>

---

### ***Decifrando uso das Crases( \`\` )***

O uso de **Crase ( \` \` )**   server para dar PRECEDÊNCIA na execução de uma comando no **SHELL**
podendo ser utilizado também com só **Parenteses ()**

Significado de **precedência** = é a ordem determinada para execução de uma expressão. 

**Exemplo:**

```shell
# comando
uname -n # exibe o host (nome do computador)

echo O nome do meu computador é uname -n
$ O nome do meu computador é uname -n # return

echo O nome do meu computador é `uname -n`
$ O nome do meu computador é TI-Support # return

echo O nome do meu computador é $(uname -n)
$ O nome do meu computador é TI-Support # return
```
<br>
**CUIDADO COM AS PEGADINHAS!**
Apesar de <code><b>` `</b></code>  e <code><b>(  )</b></code> serem utilizados para dar precedência em uma execução,  utilize-os nos lugares corretos para obter resultados desejados, **exemplo:**

```sh

# declaração de variáveis.
var1=3; var2=var1

# obtendo resultados
echo $( echo "$var2" ) # saída "var1"
echo `echo "$var2"` # saída "var1"

echo "$`echo $var2`" # saída "$va1" (string) 

echo "${!var2}" # saída 3 (value var1) - "parameter expansion"
eval echo "$`echo $var2`" # saída 3 (value var1) - "command eval"
```

 **`${!var2}`** [[Parameter expansion#^732513|Expansão de parâmetros (obtendo valor direto de outra varável como valor)]]
<br>
***Lista de precedência de operadores***

| Operador      | O que significa                                      |
| ------------- | ---------------------------------------------------- |
| `()`          | precedência de                                       |
| `~`           | complemento                                          |
| `!`           | negação                                              |
| `*/ %`        | multiplicar, dividir, modulo                         |
| `+ -`         | add, subtract                                        |
| `<< >` `>`    | turno de esquerda, turno certo                       |
| `<= >= < >`   | operadores relacionais                               |
| `== != =~ !~` | comparação de sequência / correspondência de padrões |
| `&`           | bitwise `AND`                                        |
| `^`           | bitwise exclusivo `OR`                               |
| `\|`          | bitwise inclusivo `OR`                               |
| `&&`          | lógico `AND`                                         |
| `\|`          | lógico `OR`                                          |

> Obs:. ***ATENÇÃO*** ao invocar variáveis com atribuidas com comandos, pois dependendo do valor retornado é necessario invocalas  utilizando as **ASPAS ( " " )**, devidos as espaços em branco e tabulações utilizado no retorno do comando.

**Exemplo:**

```shell
# Atribuição de variável
Arq=$(ls -l)
echo $Arqs 
total 48 drwxr-xr-x 2 britos britos 4096 Jun 4 09:31 Desktop drwxr-xr-x 6 britos britos 4096 Aug 13 09:08 Documents drwxr-xr-x 5 britos britos 4096 Aug 14 11:50 Downloads # return 

echo "$Arqs"
total 48
drwxr-xr-x  2 britos britos 4096 Jun  4 09:31 Desktop
drwxr-xr-x  6 britos britos 4096 Aug 13 09:08 Documents
drwxr-xr-x  5 britos britos 4096 Aug 14 11:50 Downloads
# return, porém organizados
# Isso acontece devido as espaços, tabs e enter "variávle $IFS"
```
<br>

---

### ***Tabela de Metacaracteres do Shell***

| Caractere           | Nome / Uso                       | Exemplo                   | Resultado                                  |
| ------------------- | -------------------------------- | ------------------------- | ------------------------------------------ |
| (espaço / tab)      | Separar argumentos               | `ls -l /etc`              | `ls` recebe dois argumentos: `-l` e `/etc` |
| `*`                 | Coringa (0 ou mais caracteres)   | `ls *.txt`                | Lista todos os `.txt`                      |
| `?`                 | Coringa (1 caractere)            | `ls arq?.txt`             | `arq1.txt`, `arqa.txt`...                  |
| `[ ]`               | Conjunto/intervalo               | `ls arq[1-3].txt`         | `arq1.txt`, `arq2.txt`, `arq3.txt`         |
| `{ }`               | Expansão de lista                | `echo {a,b,c}.txt`        | `a.txt b.txt c.txt`                        |
| `' '`               | Aspas simples (literal)          | `echo '*'`                | Saída: `*`                                 |
| `" "`               | Aspas duplas (permite variáveis) | `echo "$HOME"`            | Mostra o caminho do home                   |
| `\`                 | Escape (tornar literal)          | `echo \*`                 | Saída: `*`                                 |
| `~`                 | Diretório home                   | `cd ~`                    | Vai para `/home/usuario`                   |
| `$`                 | Variáveis e substituições        | `echo $USER`              | Mostra o usuário atual                     |
| `` ` ` `` ou `$( )` | Substituição de comando          | `echo $(date)`            | Mostra a data atual                        |
| `                   | `                                | Pipe (encadear comandos)  | `ls                                        |
| `>`                 | Redirecionar saída (sobrescreve) | `ls > arq.txt`            | Salva lista em `arq.txt`                   |
| `>>`                | Redirecionar saída (acrescenta)  | `echo oi >> arq.txt`      | Adiciona ao final                          |
| `<`                 | Redirecionar entrada             | `wc -l < arq.txt`         | Conta linhas de `arq.txt`                  |
| `&`                 | Executar em background           | `sleep 10 &`              | Não bloqueia o terminal                    |
| `;`                 | Separar comandos                 | `echo oi ; echo tchau`    | Executa os dois comandos                   |
| `&&`                | Executa próximo se sucesso       | `mkdir pasta && cd pasta` | Só entra se `mkdir` funcionar              |
| `                   |                                  | `                         | Executa próximo se falha                   |