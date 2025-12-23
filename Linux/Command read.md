O **`read`** é um comando interno do **shell (bash, sh, etc.)** que serve para **ler a entrada do usuário** (normalmente pelo teclado, mas também pode vir de arquivos ou redirecionamentos).  
Ele armazena o que o usuário digitou em uma ou mais **variáveis**.

## Observações importantes

- Se não informar variáveis, o `read` armazena a entrada na variável especial chamada **`REPLY`**.    
- O `read` **quebra a entrada em palavras** usando espaço, tabulação e Enter como separadores (a não ser que você modifique o IFS).    
- É um dos comandos mais usados para criar **scripts interativos**.

**Sintaxes:**

```sh

read [options] variable1 variable2 ...

```

**Explicação:**
- Se você informar **apenas uma variável**, toda a linha digitada será armazenada nela.    
- Se informar **várias variáveis**, o `read` divide a entrada pelos espaços:
    -  A primeira palavra vai para a primeira variável.
    -  A segunda palavra vai para a segunda variável.
    -  A última variável recebe **o restante** do que sobrar.
<br>
**Exemplo simples:**

```sh

# única variável
echo -n "Digite seu nome: "; read NAME
echo $NAME # return NAME digitado.

# or

echo -n "Digite seu nome: "; read
echo $REPLY # "variável especial"

# Varias variáveis
echo -n "Digite seu nome e idade: "; read name age
echo "name: $name, age: $age" # return name and age digitados.

```
<br>
## Opções mais usadas do `read`

- **`-p`** → mostra um **prompt** na mesma linha

```sh
read -p "Digite seu nome: " name
echo $name

```
<br>
- **`-s`** → oculta o que o usuário digitar (útil para senhas)

```sh
read -s -p "Digite sua senha: " password
echo -e "\nSenha capturada!"

```
<br>
- **`-n N`** → lê apenas **N caracteres**, sem precisar apertar Enter

```sh

read -n 1 -p "Pressione uma tecla: " tecla
echo -e "\nVocê pressionou: $tecla"

```
<br>
- **`-t SEGUNDOS`** → define um **tempo limite** (timeout)

```sh

if read -t 5 -p "Digite algo em até 5 segundos: " resposta
then
    echo "Você digitou: $resposta"
else
    echo "Tempo esgotado!"
fi

```
<br>
- **`-a`** → lê os valores em um **array**

```sh

echo "Digite três frutas:"
read -a frutas
echo "A primeira fruta foi: ${frutas[0]}"
echo "Todas as frutas: ${frutas[@]}"

```
<br>



