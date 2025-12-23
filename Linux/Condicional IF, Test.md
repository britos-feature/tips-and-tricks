    ## [[#^b59387|IF]],

# **_Instruções Shell_**

No **shell Linux**, uma **instrução** é basicamente um **comando ou conjunto de comandos** que você digita para o interpretador de comandos (como o **bash**, **zsh**, etc.) executar.

**Exemplo:**

```sh
# Instrução para criação folder(diretório) e file(arquivo)
mkdir myFolder && touch myFile
```

## **_Condição (condicional) Shell_**

Command **_`test`_**

No contexto do **shell Linux (e programação em geral)**, uma **condição** é uma **expressão lógica** que pode resultar em **verdadeiro (0)** ou **falso (≠0)**.

Ou seja, é uma **verificação que define se um comando ou bloco de comandos deve ou não ser executado**.

**Exemplo:**

```sh
# Condição, testando arquivo se existe e é arquivo comum (test)
test -f myFile.txt

echo $?
# - Se for verdadeiro, retorna `0`.
# - Se for falso, retorna `1`.
```

**ou**

```sh
# Condição, testando arquivo se existe e é arquivo comum []
[ -f myFile.txt ]

echo $?
# - Se for verdadeiro, retorna `0`.
# - Se for falso, retorna `1`.
```

**ou**

```sh
# Condição, testando arquivo se existe e é arquivo comum
[ -f myFile.txt ] && echo existe # para true

[ -f myFile.txt ] || echo não existe # false
```

## \*\*\*Maps de conditional no Shell

- ### **Options para condições com arquivo (file)**

| Teste        | Significado                                          |
| ------------ | ---------------------------------------------------- |
| `-e arquivo` | Verdadeiro se o arquivo existir (qualquer tipo).     |
| `-f arquivo` | Verdadeiro se existir e for **arquivo comum**.       |
| `-d arquivo` | Verdadeiro se existir e for **diretório**.           |
| `-r arquivo` | Verdadeiro se o arquivo for **legível**.             |
| `-w arquivo` | Verdadeiro se o arquivo for **gravável**.            |
| `-x arquivo` | Verdadeiro se o arquivo for **executável**.          |
| `-s arquivo` | Verdadeiro se o arquivo existir e **não for vazio**. |

**Exemplo:**

```sh
[ -e myFile.txt ]
echo $?

# - Se for verdadeiro, retorna `0`.
# - Se for falso, retorna `1`.
```

- ### **Options para condições com números (number)**

| Teste       | Significado                            |
| ----------- | -------------------------------------- |
| `n1 -eq n2` | Igual a (`equal`).                     |
| `n1 -ne n2` | Diferente de (`not equal`).            |
| `n1 -gt n2` | Maior que (`greater than`).            |
| `n1 -lt n2` | Menor que (`less than`).               |
| `n1 -ge n2` | Maior ou igual a (`greater or equal`). |
| `n1 -le n2` | Menor ou igual a (`less or equal`).    |

**Exemplo:**

```sh
age=50

[ "$age" -eq 51 ]
echo $?

# - Se for verdadeiro, retorna `0`.
# - Se for falso, retorna `1`.
```

- ### **Options para condições com "strings" (text)**

| Teste          | Significado                                                               |
| -------------- | ------------------------------------------------------------------------- |
| `str1 = str2`  | Verdadeiro se forem **iguais**.                                           |
| `str1 != str2` | Verdadeiro se forem **diferentes**.                                       |
| `-z str`       | Verdadeiro se a string for **vazia**.                                     |
| `-n str`       | Verdadeiro se a string **não for vazia**.                                 |
| `-v VAR`       | Verdadeiro se variável estiver sido definida **com ou sem valor**, (null) |

**Exemplo:**

```sh
# Variable
VAR=

# Testando se variável existe (mesmo que vazia)
[ -v VAR ]
echo "$?"

# - Se for verdadeiro, retorna `0` (variavel definida).
# - Se for falso, retorna `1` (variavel indefinida = não criada).
# OBS: Nesse tipo de "test" o nome da variável não se utiliza-se de $ (cifrão)

#-----------------------------------------

# Variable
NAME=""

# Success variável empty(vazia) ou mesmo null("") ( -z )
[ -z "$NAME" ]
echo "$?"

# - Se for verdadeiro, retorna `0`, se variavel estiver sem valor atribuido ou null ("").
# - Se for falso, retorna `1`, se variavel estiver com valor atribuido


# Success variável with value ( -n )
NAME="value"

[ -n "$NAME" ]
echo "$?"

# - Se for verdadeiro, retorna `0`. se variavel estiver com valor atribuido
# - Se for falso, retorna `1`, se variavel estiver sem valor atribuido ou null ("").

```

<br>

- ### **Options para condições lógicas**

| Operador       | Exemplo            | Significado                            |
| -------------- | ------------------ | -------------------------------------- |
| `!`            | `[ ! -f arquivo ]` | **NÃO** (negação).                     |
| `-a` (ou `&&`) | `[ -f a -a -r a ]` | **E** (as duas devem ser verdadeiras). |
| `-o` (ou `     |                    | `)                                     |

```sh
# Condição "Negação"
[ ! -f "dados.txt" ] && echo "O arquivo NÃO existe."

# -a Condição true se as duas condições forem true
[ -f myFile.txt -a -r myFile2.txt ] && echo true

# -o Condição true se umas das condições forem true
[ -f myFile.txt -o -f fileInexistent.txt] && echo true

# || Condição true se umas das condições forem true
[ -f myFile.txt ] || [ -f fileInexistent.txt] && echo true

[[ -f myFile.txt || -f fileInexistent.txt ]] && echo true

# OBS:. Comando + && → server para executar algo apenas se o comando anterior teve sucesso.

```

<br>
***No shell script** (especialmente em `bash` e derivados), `[]` e `[[]]` são **alias** para o comando `test`, usados para execução de testes lógicos (avaliações de expressões, como:  string, números e arquivos)* .

**OBS:. Colchetes `[ ], [[ ]]`, não serve para executar comandos**.

**_Principais diferenças entres " \[ ] " e " \[ \[ ] ] ":_**

🔹 **`[]`** (test clássico, POSIX)

- É apenas um **atalho para o comando `test`**.
- Funciona em qualquer shell compatível com POSIX (sh, dash, bash, ksh, etc.).
- Precisa de **aspas em variáveis** para não quebrar se estiverem vazias ou contiverem espaços.
- Não suporta **regex**.
- Não suporta **operadores lógicos internos** (`||`, `&&`), é preciso usar fora da expressão.
- Pode sofrer **expansão de globbing** (`*`, `?`) se não houver aspas.

      **Exemplo:**  _` "$SUPER" = "root" ] && echo "é root"`

  <br>
  🔹 **`[[]]`** (test estendido, Bash/Zsh/Ksh)

- É uma **keyword especial do shell**, não é um comando externo.
- Disponível em shells modernos (bash, zsh, ksh), mas **não é POSIX**.
- **Dispensa aspas** em variáveis, não quebra com valores vazios ou com espaços.
- Suporta **regex** com `=~`.
- Permite **operadores lógicos internos** (`||`, `&&`, `!`).
- Não expande curingas (`*`, `?`) como globbing → compara literalmente.

  **Exemplo:** _`[[ $USER = root || $UID -eq 0 ]] && echo "é root"`_
  _`[[ "arquivo.txt" =~ \.txt$ ]] && echo "é um txt"`_
  <br>

## \*\*\*Instrução utilizando " IF "

^b59387

Mandamentos do **IF** no Shell !

- **IF, não** testa **condição**, **IF**, testa **instrução**
- Comando **test**, esse sim testa **condição.**
- Para o **IF** testar condição no **SHELL**, use ele junto com o comando **test**.

A instrução **" IF "** pode ser escrito de formas diferentes:

**Opções**

- command **`condição`** direta `if grep -q ^root /etc/passwd`
- utilizando o command **`test`** `test -f arq.txt`
- com **`[]`** → mais antigo, compatível com qualquer shell. `[ -f arq.txt ] && echo "True"`
- com duplos **`[[]]`** → mais moderno, seguro, **_aceita regex_**, melhor para strings.
- com duplas **`(())`** → ideal para matemática.

**Exemplo, condição direta:**
**comando** **_"GREP"_**

```shell
if grep -q ^root /etc/passwd  # -q Quiet não retorna o item encontrado
	then echo "root encontrado!"
	else echo "root não encontrado!"
fi

$ root encontrado!
```

**Exemplo , utilizando test**

```shell
if test -d newFolder
	then
		cd newFolder
	else
		mkdir newFolder
		cd newFolder
fi
```

\*\*Exemplo, utilizando `[]` (modo mais antigo)

```shell
if [ -d newFolder ]
	then
		cd newFolder
	else
		mkdir newFolder
		cd newFolder
fi
```

\*\*Exemplo, utilizando `[[]]` (mais moderno, seguro, aceita regex, melhor para strings).

```shell
# Atribuição de variável
hh=15

# Testando instrução
if [[ $hh == [01][0-9] || $hh == 2[0-3] ]]
	then echo range entre 00 a 23
	else echo range acima de 23
fi

$ range entre 00 a 23
```

**Exemplo, utilizando sem " IF " só com os `[[]]` (\+\+ Modo melhorado ainda).**

```shell
# Atribuição de variável
hh=19

[[ $hh == [01][0-9] || $hh == 2[0-3] ]] && echo true || echo false
$ true # nesse o retorno é true

hh=24
[[ $hh == [01][0-9] || $hh == 2[0-3] ]] && echo true || echo false
$ false # nesse o retorno é false

# && esse tipo de test correponde a if then
# || esse tipo de test correponde a if else
```

**Exemplo, utilizando REGEX "`[[ =~ ]]`",** - básico
_Pode ser utilizado com **IF** ou sem eles._

```sh
name=Alex

# ^, corresponde ao inicio da string
if [[ $name =~ ^Ale ]]; # com IF
	then echo true
	else echo false
fi

lastname="Nascimento Brito"

# $, corresponde ao final da string
[[ $lastname =~ "Brito"$ ]] && echo true || echo false # sem IF

```

**Exemplo, utilizando `(())` - expressões aritméticas**
Usado só para **números**.  
Ele entende [[#^4a1aa6|operadores matemáticos]] diretamente, sem `-lt`, `-gt` etc.

```sh
value=10

if (( value > 5 ));
	then echo "Maior que 5";
	else echo "Menor que 5";
fi

# Podendo ser utilizada também com INCREMENTO
(( value++ ))
echo $value # return 11

```

**Exemplo, combinando expressões - usando **`&&`** (E lógico) e **`||`\*\* (OU lógico), parecido com outras linguagens.

- **Usando `&&` (E lógico)**

```sh
x=10
y=20

if (( x > 5 && y < 30 )); then
  echo "As duas condições são verdadeiras"
fi
```

- **Usando `||` (OU lógico)**

```sh
x=3
y=50

if (( x < 5 || y < 30 )); then
  echo "Pelo menos uma condição é verdadeira"
fi
```

- **Usando `&&` e `||` juntos**

```sh
x=15
y=40
z=5

if (( (x > 10 && y < 50) || z == 0 )); then
  echo "Condição composta satisfeita"
fi

# primeiro avalia o grupo `(x > 10 && y < 50)`, e
# depois o compara com `|| z == 0`.
```

- **Uso fora do `if` (curto-circuito)**

```sh
x=10

(( x > 5 )) && echo "x maior que 5"
(( x < 5 )) || echo "x não é menor que 5"
```

> Diferença importante: - Dentro de `(( ))`, `&&` e `||` são operadores **lógicos aritméticos**. - Fora de `(( ))`, `&&` e `||` servem para **controlar execução de comandos**.

**IMPORTANTE**
_Por baixo do panos **IF**, apenas testa a variável **[[Linux/Variable_Linux#^f60fc6|$?]]**, ja que **IF** testa instruções!_
