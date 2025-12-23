### **_Expansão de parâmetros Bash_**

No **Bash**, _expansão de parâmetros_ é o mecanismo que permite manipular variáveis de várias formas **sem precisar de comandos externos**.

**_Formas básicas de expansão_**

```sh

# variável
name="Britos"

# expansão simples
echo $name # return Britos

$ echo ${name} # mesma coisa, mas permite combinações
# return Britos

```

<br>
#### ***Valores padrão e substituição***

- **`${var:-palavra}`** → Usa _palavra_ se `var` não estiver definida ou vazia.
- **`${var:=palavra}`** → Igual ao anterior, mas atribui o valor a `var`.
- **`${var:?mensagem}`** → Erro com _mensagem_ se `var` não estiver definida/vazia.
- **`${var:+palavra}`** → Usa _palavra_ se `var` estiver definida e não vazia.

\*\*\*Exemplo utilizando padrão de substituições:

- **`${var:-value}`**
  Comando que "**retorna o value**" se a variável **não estiver nenhum valor atribuído ou estiver vazio**, <span style="color:red">caso ja tenha algum valor já atribuído seu <b>"retorno é o próprio valor</b>"</span>.

```sh

# apagando variável (caso exista)
unset name
echo $name # return empty (vazio)

echo ${name:-"Britos"} # return Britos (mais não atribuido a variável)
echo $name # return empty (variável continua vazia)

```

<br>
- **`${var:=value}`**
Comando que "**return o value**" se a variável **estiver vazia, atribuindo o valor a variável**,   <span style="color:red">caso ja tenha algum valor já atribuído seu <b>"retorno é o próprio valor</b>"</span>

```sh

# apagando variável (caso exista)
unset name
echo $name # return empty (vazio)

echo ${name:="Britos"} # return Britos (atribui o valor a variável)
echo $name # return Britos

```

<br>
**`${var:?msg}`**
Comando que "**retorna mensagem error**" personalizada se a variável estiver vazia, <span style="color:red">caso ja tenha algum valor já atribuído seu <b>"retorno é o próprio valor</b>"</span>

```sh

# apagando variável
unset name
echo $name # return empty (vazio)

echo ${name:?"Error empty variable"}
echo $name # return bash: name: Error empty variable

```

<br>
**`${var:+value}`**
Comando que "**retorna  value**" se a variável já estiver atribuída e não vazia,  <span style="color:red">caso ja tenha algum valor já atribuído seu <b>"retorno é o próprio valor</b>"</span>

```sh

# apagando variável
unset name
echo $name # return empty (vazio)

echo ${name:+"value"} # return empty (variável não atribuida)
echo $name # return empty

```

<br>
#### ***Comprimento de variáveis***
Retorna a quantidade de caractere contido no valor da variável.

```sh

# variável
name=value
echo ${#name} # return 5 (quantidade de caractere)

```

<br>
#### ***Substring***
Retorna uma parte da string a partir de, conforme descriminado, <span style="color:red">mais não altera seu  valor permanecendo o valor atribuído</span>

```sh

name="Alex"
echo ${name:3} # return xandre
echo ${name:0:3} # return Ale

```

<br>
#### ***Remoção de prefixo/sufixo***
Retorna o prefixo, sufixo da variável, conforme determinado, <span style="color:red">mais não altera seu  valor permanecendo o valor atribuído</span>

```sh

# variável
bkpFile="backup.tar.gz"

echo ${bkpFile#*.} # return "tar.gz"
echo ${bkpFile##*.} # return "gz"
echo ${bkpFile%.*} # return "backup.tar"
echo ${bkpFile%%.*} # "backup"

```

<br>
#### ***Substituição de padrões***
Retorna a variável com seu padrão substituído, conforme descriminado, <span style="color:red">mais não altera seu  valor permanecendo o valor atribuído</span>

```sh

# variável
name="Alex"

echo ${name/e/E} # return Alex (substitui a primeira ocorrência "e")
echo ${name//e/E} # return Alex (substitui todas ocorrência "e")
echo ${name/#A/a} # return alex (substitui apenas se for ínicio)
echo ${name/%e/E} # return alex (substitui apenas se for fim)

```

<br>
#### ***Transformações de caso (case modification)***
- **`${var^}`** → primeira letra maiúscula
- **`${var^^}`** → todas maiúsculas
- **`${var,}`** → primeira letra minúscula
- **`${var,,}`** → todas minúsculas
- **`${var~}`** → inverte o caso da primeira letra
- **`${var~~}`** → inverte o caso de todas

```sh

# variável
name=alex
echo ${name^} # return Alex (first upper)
echo ${name^^} # return ALEX (all upper)

# variável
name=ALEX
echo ${name,} # return alex (first lower)
echo ${name,,} # return alex (all lower)

# variável
name=Alex
echo ${name~} # return alex (inverte first caractere)
echo ${name~~} # return alex (inverte all caractere)

```

<br>
#### ***Expansão Direta variável***
Utiliza o valor de uma variável com nome para outra variável.

```sh

# variáveis
name="Alex"
Alex="Brito"

echo ${!name} # return Brito

# variável (prefixo)
bkpfile1="one"
bkpfile2="two"
bkpfile3="three"

# utilização prefixo return variable
echo ${!bkp*}; echo ${!bkp@} # return bkpfile1 bkpfile2 bkpfile3

```

^732513

<br>
#### ***Manipulação de Arrays***
Criação e manipulação de array no bash

```sh

# variable ARRAY
arr="one" "two" "three"

echo ${arr[0]} # return "one"
echo ${arr[1]} # return "two"
echo ${arr[2]} # return "three"

# Return All element do ARRAY
echo ${arr[@]} # return "one" "two" "three"
echo ${arr[*]} # return "one" "two" "three"

# Return quantidade de elementos do ARRAY
echo ${#arr[@]} # return 3 (quantidade de elementos)

# Return elementos selecionados (slice) do ARRAY
echo ${arr[@]:1:2} # return "two" "three"

# Adciona elementos ao ARRAY
arr+="four" "five"
echo ${arr[@]} # return "one" "two" "three" "four" "five"

# iterando no ARRAY
for x in "${arr[@]}"; do
  echo "[$x]"
done
# return ["one"] ["two"] ["three"] ["four"] ["five"]

```

<br>
#### ***Manipulação de array associativo (chave: value)***
Criação e manipulação de arrays associativos (chaves e valores)
Para se declarar ARRAYS associativos e necessário utilizar o comando `declare -A`

- **_Declaração de ARRAY associativos_**

```sh

# varieble ARRAY
declare -A states

# Array associativos (chaves: values)
states[SaoPaulo]="Sao Paulo"
states[RioJaneiro]="Rio de Janeiro"
states[MinasGerais]="Belo Horizonte"

```

<br>
- ***Acessando Values***

```sh

echo ${states[SaoPaulo]} # return "Sao Paulo"
echo ${states[RioJaneiro]} # return "Rio de Janeiro"
echo ${states[MInasGerais]} # return "Belo Horizonte"

```

<br>
- ***Listando todas as chaves***

```sh

echo ${!states[@]} # return "SaoPaulo" "RioJaneiro" "MinasGerais"

```

<br>
- ***Listando todos os values***

```sh

echo ${states[@]} # return "SaoPaulo" "Rio de Janeiro" "Belo Horizonte"

```

<br>
- ***Quantidade de pares chave→valor***

```sh

echo ${#states[@]}   # 3

```

<br>
- ***Iterando sobre chave e valor***

```sh

for value in "${!states[@]}"; do
  echo "States: $states → Capital: ${state[$value]}"
done

# Return
# States: SaoPaulo -> Capital: "Sao Paulo"
# States: RioJaneiro -> Capital: "Rio de Janeiro"
# States: MinasGerais -> Capital: "Belo Horizonte"

```

<br>
- ***Adicionando e removendo ARRAY associativos***

```sh

states[Bahia]="Salvador"  # add ARRAY associativo
unset states[Bahia] # delete ARRAY associativo

```

<br>
