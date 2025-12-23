	
# Shell Linux - variables

No Shell, as variáveis podem ser classificadas em **variáveis locais**, **variáveis globais** e **variáveis de ambiente**.


## 1. ***Variáveis Locais***

Variáveis locais **são aquelas definidas dentro de um shell ou processo específico e só são acessíveis nesse contexto.** Ou seja, elas são limitadas ao escopo da sessão ou script onde foram definidas. 
Se você fechar o terminal ou o script, a variável local será destruída e seu valor se perderá.

```sh
# Criando variáveis local
var="value"
```

> **`variable_local`** é uma variável local que só pode ser usada dentro do script e no escopo da sessão (terminal atual).

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## 2. ***Variáveis Globais*** 
(Definidas no shell e disponíveis para subprocessos)

Uma variável global é definida de forma semelhante à local, mas você **precisa exportá-la** para torná-la **acessível em outros processos filhos ou scripts.**

```sh
# Criando variáveis Globais

var_global="value"
export variable_global

# or

export var_global="value"
```

> **`variable_global`** será acessível em qualquer script ou processo iniciado dentro do shell atual.

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## 3. ***Variáveis de Ambiente***

Essas variáveis podem ser usadas por qualquer programa ou comando executado no shell e são amplamente utilizadas para configurações do sistema, como caminhos de diretórios e preferências de usuários.

```sh
# Criando variáveis environment (ambiente)

export PATH=$PATH:/novo/diretorio

```

Aqui, a variável `PATH` é uma variável de ambiente, e estamos acrescentando um novo diretório à lista de diretórios que o sistema deve procurar para localizar executáveis.

- **Exemplo de variáveis de ambiente mais comuns**:
    
    - **`HOME`**: Diretório inicial do usuário.
    - **`USER`**: Nome do usuário logado.
    - **`PATH`**: Diretórios onde o shell procura por executáveis.
    - **`PWD`**: Diretório atual.
    - **`SHELL`**: Caminho para o shell em uso.


---
---

### ***Resumo:***

- **Local**: A variável existe apenas no script ou terminal onde foi criada. Não afeta outros processos ou scripts.
    
- **Global**: Está disponível para o shell atual e subprocessos iniciados a partir dele. Para ser visível em subprocessos, você precisa exportá-la.
    
- **De Ambiente**: São variáveis especiais que definem o ambiente de execução do sistema ou de um processo. Elas são frequentemente usadas para configurações como caminhos e preferências do usuário.

---

%% ************************************************************************************************************//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## ***Atribuição variáveis -> Variable Linux***

***Regras para atribuições de variáveis no linux***

- **Iniciar com uma letra ou sublinhado (`_`)**: O nome de uma variável deve começar com uma letra (a-z, A-Z) ou um sublinhado (`_`). Não pode começar com um número.
-  **Caracteres permitidos**: Após o primeiro caractere, o nome da variável pode conter letras, números e sublinhados (`_`).
- **Case-sensitive**: O Shell diferencia maiúsculas de minúsculas, então `variavel` e `VARIAVEL` são variáveis diferentes.
- **Evitar palavras reservadas**: Não utilize nomes de variáveis que sejam palavras-chave do Shell ou comandos do sistema, como `if`, `then`, `else`, `for`, `while`, etc.
- **Usar letras maiúsculas para constantes**: Uma convenção comum é usar letras maiúsculas para nomes de variáveis que são "constantes" ou não mudam durante o script.
- **Usar letras minúsculas para variáveis comuns**: Para variáveis que podem ser alteradas ou são temporárias no script, é comum usar letras minúsculas.


**Exemplo:**

```sh
# Atribuição/ declaração de variáveis

name=John
lastname=Christler
age=38
home_diretory="/home/john"

echo "Name $name $lastname, age: $age my diretory: $home_diretory."

```
<br>
### ***Formas de criar variáveis no Shell***

| Método                             | Exemplo                      | Explicação / Quando usar                                  |
| ---------------------------------- | ---------------------------- | --------------------------------------------------------- |
| **Atribuição simples**             | `nome="Maria"`               | Forma mais comum. Sempre sem espaços em torno de `=`.     |
| **Com aspas duplas**               | `msg="Olá $nome"`            | Expande variáveis dentro da string.                       |
| **Com aspas simples**              | `msg='Olá $nome'`            | Não expande variáveis, literal.                           |
| **Múltiplas atribuições**          | `a=1 b=2 c=3`                | Define várias variáveis na mesma linha.                   |
| **Substituição de comando**        | `data=$(date)`               | Armazena saída de um comando. Forma moderna.              |
| **Substituição com crases**        | `` data=`date` ``            | Forma antiga, ainda suportada.                            |
| **Aritmética com `let`**           | `let x=5+3` / `let x++`      | Operações numéricas (forma mais antiga).                  |
| **Aritmética com `(( ))`**         | `(( x = 5 + 3 ))`            | Mais moderno e legível que `let`. Preferido.              |
| **Aritmética com `expr`**          | `x=$(expr 5 + 3)`            | Muito antigo. Só por compatibilidade.                     |
| **Leitura de usuário (`read`)**    | `read -p "Nome: " nome`      | Pega valor digitado pelo usuário no terminal.             |
| **Exportar para subprocessos**     | `export PATH=$PATH:/meu/bin` | Torna variável disponível em comandos/filhos.             |
| **Atribuição inline para comando** | `DEBUG=1 ./script.sh`        | Variável só vale durante a execução desse comando.        |
| **Com `declare`**                  | `declare -i num=10`          | Define tipo (inteiro, readonly, array, etc). Mais formal. |
| **Com `typeset`**                  | `typeset -r constante=123`   | Igual a `declare`, mas usado em outros shells também.     |
| **Variáveis de ambiente (`env`)**  | `env VAR=valor comando`      | Define variável só para execução do comando.              |
| **Argumentos de script**           | `$1`, `$2`, `$#`, `$@`       | Usados para pegar os parâmetros passados ao script.       |
| **Subshells / pipelines**          | `linhas=$(ls                 | wc -l)`                                                   |

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## ***Arrays assignment (declaração/ atribuição)***
No **Shell**, a forma de declarar e usar arrays depende do **tipo de shell** que você está usando.

**Sintaxes:**

```sh

# basic
myArray=("item1" "item2" "item3")

echo "${myArray[0]}" # return item1

# Array utilizando "declare" (opcional, mas recomendado)
declare -a myArray=("item1" "item2" "item3") # opção "-a" indica que é um array indexado numericamente.

echo "${myArray[1]}" # return item2

# Array associativo
# Um array associativo usa chaves em vez de índices numéricos:
declare -A myArray
myArray[red]="#ff0000"
myArray[green]="#00ff00"
myArray[blue]= "0000ff"

echo "${myArray[red]}" # return "#ff0000"

```
<br>
**OBS:**
- Os valores podem ser strings ou números.
- Não precisa declarar tamanho antes.
<br>
---
---

####  **Manuseando ARRAYS**
 
  - ***Acess element***

```sh

echo "${myArray[0]}"   # first element
echo "${myArray[1]}"   # second element

# Associativos
echo "${myArray[value]}" # return o value

```
<br>
- ***Add elements

```sh

# basic
myArray+=("item4")

# Associativos
myArray[value]="item4"

```
<br>
- ***Quantidade/ tamanho de elementos do Array***

```sh

echo "${#myArray[@]}" # return quantidade de elementos dentro do "array"

echo "${#myArray[2]}" # return tamanho do "array" especificado. 

```
<br>
- ***Listar elementos/ chaves do "array"

```sh

echo "${myArry[@]}" # return all elements

echo "${!myArray[@]}" # return all índices (ou chaves no associativos)

```
<br>
- ***Remove elements***

```sh

unset myArray[value] # remove element especificado.

unset myArray[chave] # remove element especificado (associativo)

```
<br>
- ***Percorrer "array indexado"***
	- ***Value*** *Elements*

```sh

for value in "${myArray[@]}"; do
	echo "$value"

```
<br>
	- ***Index*** *Elements*

```sh

for index in "${!myArray[@]}"; do
	echo "$index --- ${myArray[$index]}"
done

```
<br>
- ***Percorrer "array associado"

```sh

for key in "${!myArray[@]}"; do 
	echo "$key --- ${myArray[$key]}"
done

```
<br>
- ***Copiar arrays***

```sh

newArray=("${myArray[@]}")

```
<br>
- ***Concatenar arrays***

```sh

array1=("a" "b" "c")
array2=("d" "e" "f")

array3=("${array1[@]}" "${array2[@]}")

echo "${array3[@]}" # return "a b c d e f"

```
<br>
- ***Sub-arrays / Fatiamento***

```sh

myArray=("a" "b" "c" "d" "e" "f")

echo "${myArray[@]:1:3:}" # return "b" "c" "d" "e" "f"
# :1:3 = inicia / termina (fatiamento)

```
<br>

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## ***Command to activate script***
***Tornar o Arquivo Executável*** (permissões de execução para o script)

```sh
# Comando para permissão de execução do arquivo
chmod +x nameScript.sh

# Executando o arquivo
./nameScript.sh
```

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## ***Variable List (local, environment)***
Comando para listar variable (local, ambiente) e funções definidas no **SHELL**

### **`env`** (Environment)

- **Objetivo:** O comando `env` é utilizado para exibir o **ambiente de execução** atual, incluindo as variáveis de ambiente, ou para **executar um comando em um ambiente modificado** (com variáveis de ambiente temporárias).
    
- **Uso principal:**

	- Exibir as variáveis de **ambiente do shell atual**.
    - Modificar o ambiente antes de executar um comando (por exemplo, alterar temporariamente uma variável de ambiente e executar um comando com essa alteração).
        

**Comando sem argumentos:**  
    Quando executado sem argumentos, `env` lista todas as variáveis de ambiente do shell.
	
```sh
env
```


**Modificar variáveis de ambiente temporariamente:**  
Você pode usar `env` para alterar variáveis de ambiente temporariamente ao executar um comando. Isso **não afeta o shell atual**, apenas o comando executado.

```sh
env VARIAVEL="valor" comando
```

> Também podemos executar um comando com uma variável temporária sem alterar o ambiente global do shell:

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

### **`set`** (local, Environment)

O comando **`env`** é utilizado para exibir **todas as variáveis do shell** (variáveis de ambiente + variáveis locais) e **funções definidas**.  A saída é ordenada por nome e geralmente bem extensa.

Diferente de `env`, o **`set`** mostra também variáveis **locais** do shell e funções, não só as exportadas.


**Comando sem argumentos:**  
    Quando executado sem argumentos, `set` exibe **todas as variáveis do shell** (variáveis de ambiente + variáveis locais) e **funções definidas**.

```sh
set
```

---

**Comando para listar apenas variáveis**
Listar todas as variáveis do shell num formato mais “limpo”

```sh
( set -o posix ; set ) | less

# Explicação:
# 
# Parênteses `( ... )`
# No shell, parênteses criam um subshell:
# Tudo dentro deles roda em um processo filho separado do shell atual.
# Alterações feitas dentro não afetam o shell principal.
# 
var=10
( var=20 )
echo $var  # var=10 (não muda o valor da variável)

# -o posix, faz o shell se comportar de forma mais compatível com o padrão POSIX.
# No caso do `set`, isso muda o formato de saída:
	# Ele deixa de exibir funções.
    # Mostra só variáveis e valores de maneira simples (`NOME=valor`).
    # Sem cores, sem formatações específicas do bash.

# e `set` sem argumentos, lista todas as variáveis e funções conhecidas pelo shell. Com o POSIX ativado, a lista fica mais “pura” e sem funções no meio.
```

> “No subshell, primeiro ativa o modo POSIX, depois executa `set`.”


---

**Uso para definir variáveis:**
Você pode usar `set` para mudar os parâmetros posicionais (`$1`, `$2`...)

```sh
set one two three
echo "$1"  # one
echo "$2"  # two
echo "$3"  # three
```

> Aqui, `set` substituiu os parâmetros passados ao script/terminal.


---

**Limpar todos os parâmetros posicionais**

```sh
set --
```


**Uso para alterar opções do shell**
O **`set`** tem uma série de **opções de controle** (flags) que mudam o comportamento do shell.  

| Opção | Significado                                                                  |
| ----- | ---------------------------------------------------------------------------- |
| `-e`  | Faz o shell encerrar o script se um comando retornar erro (exit status ≠ 0). |
| `-u`  | Gera erro ao usar variáveis não definidas.                                   |
| `-x`  | Mostra cada comando executado (modo debug).                                  |
| `-o`  | Usado para ativar/desativar opções por nome (`set -o pipefail`).             |
| `+`   | Desativa uma opção (`set +x` para parar debug).                              |

```sh
set -x    # Ativa debug (mostra comandos antes de executá-los)
set +x    # Desativa debug

set -e    # Encerra script ao primeiro erro
set -u    # Erro ao usar variável não definida
```


**Diferença para `env`**

- `env` → mostra apenas **variáveis de ambiente** (as exportadas).
- `set` → mostra **variáveis locais + variáveis de ambiente + funções**.
- `export` → lista ou define apenas variáveis que serão exportadas para subprocessos

**Resumo rápido**:

- `set` sozinho → mostra tudo (variáveis e funções).
- `set [opções]` → altera o comportamento do shell.
- `set -- args...` → redefine parâmetros posicionais.
- Muito usado em scripts para depuração (`-x`), segurança (`-e`, `-u`) e consistência (`-o pipefail`).


---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---

## [[Declare|Variable List - command `declare`]]

```sh
declare -p | grep 'nameVariable'
```

---

%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%

---
## ***Variáveis Especiais no Shell Linux (Bash)***

#### ***System variables*** -> (variavéis de sistema)

As **variáveis especiais** são variáveis internas do **shell** (como o Bash, sh, zsh).  
Elas não precisam ser criadas: já existem e fornecem **informações sobre o script, argumentos, processos e estado do shell**. 


***Lista das principais variáveis de sistemas:***

| Variável        | Significado                                       | Exemplo                                          |
| --------------- | ------------------------------------------------- | ------------------------------------------------ |
| `$0`            | Nome do script ou comando executado               | `./script.sh`                                    |
| `$1`, `$2`, ... | Argumentos passados ao script (posição)           | `$1` = primeiro argumento                        |
| `$#`            | Número total de argumentos passados               | Se rodar `./script.sh a b c` → `3`               |
| `$@`            | Todos os argumentos, como lista                   | `a b c`                                          |
| `$*`            | Todos os argumentos, como única string            | `"a b c"`                                        |
| `$$`            | PID (ID do processo) do shell atual               | `12345`                                          |
| `$!`            | PID do último processo rodando em background      | `./script.sh &` → `$!` mostra PID                |
| `$?`            | Código de saída do último comando                 | `0` = sucesso, `≠0` = erro                       |
| `$_`            | Último argumento do último comando executado      | Se rodar `ls arquivo.txt` → `$_` = `arquivo.txt` |
| `$-`            | Opções atuais do shell (flags ativas)             | Ex: `himBH`                                      |
| `$$PPID`        | PID do processo pai (processo que chamou o shell) | Ex: `4321`                                       |

---

### ***Por categorias:***

- ***Argumentos passados***:

	- **`$0`** → Nome do script ou comando em execução.
	- **`$1, $2, $3...`** → Argumentos passados (posição).
	- **`$#`** → Número total de argumentos passados.
	- **`$@`** → Todos os argumentos, como **lista** (cada um separado).
	- **`$*`** → Todos os argumentos, como **string única**.

***exemplo:***

```sh
#!/bin/bash
echo "Name: $0"
echo "Qtd argumentos: $#"
echo "First: $1"
echo "Full list - (lista): $@"
echo "All (string): $*"
```

```sh
# execução do script.sh
./test.sh Linux Shell Variables

# return
Qtd argumentos: 3
First: Linux
Full list: Linux Shell Variables
All (string): Linux Shell Variables
```

---

- ***Por Processos***

	- **`$$`** → PID (process ID) do shell atual.
	- **`$!`** → PID do último processo rodando em **background**.
	- **`$PPID`** → PID do processo pai (quem chamou o script).

***exemplo:***

```sh
echo "PID atual: $$"
(sleep 10 &)   # processo em background
echo "PID último background: $!"
echo "PID pai: $PPID"
```

---

- ***Execução de comando***

	- **`$?`** → Código de saída do último comando (`0` = sucesso, `≠0` = erro).
	-  **`$_`** → Último argumento do último comando executado.

```sh
ls myFile.txt
echo "Código de saída: $?" # 0 = true, ≠0 = error
echo "Último argumento: $_" # $ myFile.txt 
```

### ***Resumindo***

- Argumentos → `$0`, `$1`, `$@`, `$#`, `$*`
- Processos → `$$`, `$!`, `$PPID`
- Execução → `$?`, `$_`
- Ambiente → `$-`

Essas variáveis são muito usadas em **scripts de automação**, **tratamento de erros** e **controle de processos**.

---

### Exemplos práticos de uso de variáveis especiais no shell

- ***Script para verificar argumentos obrigatórios (`$#`, `$1`, `$2`)***

```sh
#!/bin/bash

if [ $# -lt 2 ]; then
    echo "Uso: $0 <origem> <destino>"
    exit 1
fi

echo "Copiando de $1 para $2..."
cp -r "$1" "$2"

if [ $? -eq 0 ]; then
    echo "Cópia concluída com sucesso!"
else
    echo "Erro ao copiar arquivos."
fi
```

***Explicação***

- `$#` → verifica se o usuário passou argumentos.
- `$0` → exibe o nome do script.
- `$?` → checa se o `cp` funcionou.

---

- ***Script de backup com timestamp (`$$`, `$!`, `$PPID`)***

```sh
#!/bin/bash

backup="backup_$$.tar.gz"   # $$ = PID do processo atual
tar -czf "$backup" /etc &   # roda em background

echo "Backup iniciado em background, PID: $!"
echo "Script executado pelo processo pai: $PPID"
```

***Explicação***

- `$$` → para criar um nome único para o backup.
- `$!` → para mostrar o PID do processo em background.
- `$PPID` → para saber quem chamou o script.

---

- ***Script de histórico de comandos (`$_`)***

```sh
#!/bin/bash

echo "Rodando comando: ls /tmp"
ls /tmp
echo "Último argumento usado foi: $_"
```


