O comando **`for`** no Shell (bash, sh, zsh, etc.) é usado para criar laços de repetição, ou seja, executar um bloco de comandos várias vezes de forma automática, variando algum valor em cada iteração.

#### Para que serve?

- Automatizar tarefas repetitivas.
- Processar lotes de arquivos.
- Executar comandos sequenciais com diferentes parâmetros.
- Evitar escrever o mesmo comando várias vezes.

#### Sintaxes básica

```sh

for VAR in value_list
do
    commands...
done

```
<br>
**Explicação:**
- **`VAR`** → é uma variável temporária que assume cada valor da lista.
- **`value_list`** → pode ser uma lista escrita manualmente, um resultado de comando, ou até um _range_.
- **`commands...`** → O bloco entre **`do`** e **`done`** será repetido para cada valor da lista.
<br>
### **[[#^f41cd9|Advanced for/array]]** 

#### Alguns modos para se declarar `for`

1. **Iterando sobre uma "lista fixa"**

```sh

for color in red blue green
do
	echo "This color is: $color" # Essa cor é: $color
done

# saída
This color is: red
This color is: blue
This color is: green

```
<br>
2. **Iterando sobre um "intervalo de números"**

```sh

for n in {1..9}
do
	echo $n
done

# saída
1
2
3
...

```
<br>
3. **Iterando sobre "arquivos em um diretório"**

```sh

for arq in *.txt
do
	echo $arq
done

# Esse laço passa por todos os arquivos `.txt` do diretório atual

```
<br>
4. **Iterando sobre "array de elementos"

<br>
5. **Iterando sobre "saída de comando"**

```sh

for user in $(cat userList.txt)
do
	echo "User create: $user"
done

# Cada linha do arquivo `userList.txt` é usada como valor.

```
<br>
5. **Estilo C-like (somente no `bash`, `ksh`, `zsh`)**

**Sintaxes**

```sh
for (( initialization ; conditonal ; increment ))
do
    commands...
done

```
<br>
**Exemplo:

```sh

for (( i=1; i<=3; i++))
do
	echo $i
done

# saída
1
2
3

```

> Essa forma **não funciona no `sh`**, apenas em shells mais modernos (`bash`, `ksh`, `zsh`).


### ***Advanced example `for` and `array`***

^f41cd9

#### **Renomear arquivos em lote**

Adicionar sufixo `_old` a todos os arquivos `.log`

```sh

arquivos=( *.log )

for f in "${arquivos[@]}"; do
    mv "$f" "${f%.log}_old.log"
done

```

> **`${f%.log}`** remove a extensão `.log` antes de adicionar `_old`.
<br>
#### **Backup automático de diretórios**

```sh

pastas=( /etc /var/log /home )

for dir in "${pastas[@]}"; do
    tar -czf "backup_$(basename "$dir").tar.gz" "$dir"
done

```
<br>
#### **Processamento de logs de múltiplos serviços**

```sh

servicos=( nginx mysql sshd )

for s in "${servicos[@]}"; do
    echo "Últimas entradas de $s:"
    journalctl -u "$s" -n 5
    echo "------------------------"
done

```
<br>
#### **Array associativo: mapear usuários para pastas**

```sh

declare -A usuarios
usuarios[ana]="/home/ana"
usuarios[beto]="/home/beto"
usuarios[carla]="/home/carla"

for user in "${!usuarios[@]}"; do
    echo "Backup do usuário $user em ${usuarios[$user]}"
    tar -czf "backup_$user.tar.gz" "${usuarios[$user]}"
done

```
<br>
#### **Executar comandos em paralelo (background)**

```sh

hosts=( google.com github.com openai.com )

for h in "${hosts[@]}"; do
    ( ping -c 2 "$h" > "$h.log" ) &
done

wait  # espera todos terminarem
echo "Todos os pings concluídos!"

```
<br>
#### **Gerar combinações (nested loops com arrays)**

```sh

nomes=(Ana Beto Carla)
cargos=(Dev QA Manager)

for n in "${nomes[@]}"; do
    for c in "${cargos[@]}"; do
        echo "$n - $c"
    done
done

```

> Cria todas as combinações possíveis entre nomes e cargos.
<br>
#### **Array dinâmico a partir de saída de comando**

```sh 

arquivos=( $(find . -maxdepth 1 -type f -name "*.txt") )

for f in "${arquivos[@]}"; do
    echo "Arquivo encontrado: $f"
done

```
<br>

