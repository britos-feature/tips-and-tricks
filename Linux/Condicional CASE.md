
O **`case`** é uma estrutura de decisão usada em **scripts shell** para comparar uma variável ou expressão contra **padrões** e executar comandos de acordo com o resultado.  
<br>
***Sintaxes:***
O comando **`case`** suporta uso de **metacaracteres.**

```sh
case var in
	PADR¹) commands ;;
	PADR”) commands ;;
	*) commands;;
esac

```
<br>
**Explicação:**
- **`var`** → valor a ser testado (geralmente uma variável).
- **`padrão`** → pode usar curingas (`*`, `?`, `[ ]`, etc.) para casar com valores, <span style="color:red"><b>(não são aceitas expressões).</b></span>
- **`commands`** → instruções que serão executadas se o padrão casar.
- **`;;`** → indica o fim do bloco de comandos de um padrão.
- **`*`** → funciona como “default” (caso nenhum padrão anterior case).
- **`esac`** → finaliza a estrutura (`case` escrito ao contrário).


> Dentro de `case`, os padrões precisam ser **expressões de globbing** (`*`, `?`, `[ ]`), não expressões aritméticas ou `[[ ... ]]`
<br>

**Exemplos básico:**

```sh
#!/bin/bash
# Script

echo "Digite uma letra: "
read letra

case $letra in
    a)
        echo "Você digitou a letra A"
        ;;
    b)
        echo "Você digitou a letra B"
        ;;
    c|d)
        echo "Você digitou C ou D"
        ;;
    *)
        echo "Letra não reconhecida"
        ;;
esac

```
<br>
**Explicando:**

- Se o usuário digitar `a`, mostra mensagem correspondente.
- Se digitar `b`, executa outro bloco.
- Se digitar `c` **ou** `d`, usa o `|` para casar mais de um valor.
- Se não casar com nada, cai no `*`.
<br>
#### Uso de curingas em padrões
O `case` aceita **globbing** (padrões de correspondência):

- `*` → qualquer sequência de caracteres.
- `?` → qualquer caractere único.
- `[abc]` → casa com `a`, `b` ou `c`.
- `[0-9]` → casa com qualquer dígito de 0 a 9.
- `|` → **OU lógico (ex.: `sim|yes`).**
<br>
```sh
read arquivo

case $arquivo in
    *.txt) echo "Arquivo de texto";;
    *.jpg|*.png) echo "Arquivo de imagem";;
    ?.sh) echo "Script com nome de 1 caractere";;
    *) echo "Outro tipo de arquivo";;
esac

```

---

#### Script de exemplo, utilizando "metacaracteres"
 **globbing** (padrões de correspondência)

**script.sh**

```sh
#!/bin/bash

# variável test "Char"
read -p "Insira um(01) caractere: " Char

case $Char in
	?) : ;;
	*) echo "Error: Insira apenas um caractere"
		exit 1
esac

case $Char in
	[a-z]) echo "Caractere é uma letra minúscula: $Char" ;;
	[A-Z]) echo "Caractere é uma letra maiúscula: $Char" ;;
	[0-9]) echo "Caractere é uma numero: $Char"          ;;
	 *) echo "Caractere é um especial: $Char"            ;;
esac

```
<br>
#### Script de exemplo, utilizando "commands"

**script.sh**

```sh
#!/bin/bash

echo "Escolha uma opção:"
echo "1 - Mostrar data"
echo "2 - Listar arquivos"
echo "3 - Mostrar usuário atual"
echo "0 - Sair"

read opcao

case $opcao in
    1) date
        ;;
    2) ls -lh
        ;;
    3)  whoami
        ;;
    4) echo "Saindo..."
        ;;
    *) echo "Opção inválida"
        ;;
esac

```
<br>
#### Operadores `;&` e `;;&` (variações de fluxo) 
Os operadores **`;&`** e **`;;&`** são variações de fluxo dentro do comando **`case`**, servindo para executar os comandos do bloco atual **e continua testando ou executando o bloco seguinte**.

**Exemplo**  **`:&`** 
_(executa os comandos do bloco atual **e continua direto para o bloco seguinte**.)_

```sh
#!/bin/bash

num=2

case $num in 
	1) echo "Primeiro" ;&
	2) echo "Segundo" ;&
	3) echo "Terceiro" ;;
	*) : ;;
esac

# saída
Segundo
Terceiro

```

> Mesmo que `$num=2` não case com o `3`, o bloco do `3` é executado porque usamos `;&` no bloco 2
<br>

**Exemplo `;;&`**
_(executa o bloco atual e, em vez de sair, continua verificando os próximos padrões.)_

```sh
#!/bin/bash

num=10

case $valor in
  10)
    echo "Dez" ;;&
  [0-9]*)
    echo "Começa com número" ;;
  *) echo "Não corresponde o padrão";;
esac

# saída
Dez
Começa com número

```
<br>
**Outro exemplo com `;;&`

```sh
#!/bin/bash

num=$1   # passando o número como argumento na chamada do script

case $num in
  5)
    echo "é 5" ;;&
  8)
    echo "é 8" ;;&
  10)
    echo "é 10" ;;&
  *[02468])   # qualquer número terminando em 0,2,4,6,8
    echo "é par" ;;
  *)
    echo "não é 5, 8, 10 nem par"
    ;;
esac

```
<br>