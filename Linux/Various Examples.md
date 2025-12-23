
## Alterando variável apenas para "um" _**COMMAND**_

```sh

# Variavel de sitema "IFS" apenas para command "READ"

while IFS=: read user trash1 uid trash2; do
	(( line++));
	echo "$line - $user: $uid";
done < /etc/passwd

```

```bash
# saída

1 - root: 0
2 - daemon: 1
3 - bin: 2
...

```
<br>
%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%
<br>
## Command "READ" variável default is _**$REPLY**_

```sh
# $REPLY, utilizar-se quando não se declarada variável para de recebimento para o comando.

# declarando variável
read -p "Insert my name: " name; # declaração de variável "$name"
echo "$name"; # return da captura do "input"

# sem declaração
read -p "insert my name: ";
echo "$REPLY"; # return da captura do "input"

```
<br>
%% //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// %%
<br>

