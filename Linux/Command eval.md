O **`eval`** no shell (bash, sh, etc.) é um comando especial que **pega uma string e a avalia como se fosse um comando de shell**.  Ou seja, ele **interpreta e executa** o conteúdo da string.

**Resumindo:**
	O `eval` **pega seus argumentos**, junta tudo em **uma única string**, e depois **manda essa string de volta para o shell interpretar** como se fosse um comando digitado na linha de comando.
<br>
- ***Funcionamento básico***  `eval STRING`
	- O **shell** pega a STRING;
	- Expande variáveis, substituições de comandos, etc...;
	- Depois executa o resultado como se você tivesse digitado no terminal.vocÊ

**Exemplo simples**

```sh

# COMMAND

cmd="ls -l"
# o shell vê que cmd="ls -l"

$cmd 
# saída (shell executa command "ls -l" valido!)

eval $cmd
# eval então vira: ls -l (shell executa comando valido)

```
<br>
```sh

# STRING

greeting="Hello World"
msg="greeting"
# o shell vê que msg="greeting"

$msg 
# saída "ERROR" (shell não encontrou program "Hello World" para executa-lo)

echo ${!msg} 
# saída "Hello World" (direct value, variable "greeting")

# or
eval echo "$`echo $msg`" 
# saída "Hello World" (direct value, variable "greeting")

```
<br>
### ***Comparativo: `eval` vs alternativas seguras***

|Situação|Com `eval`|Alternativa sem `eval`|Observação|
|---|---|---|---|
|**Executar comando guardado em string**|`bash\ncmd="ls -l"\neval $cmd\n`|`bash\ncmd=(ls -l)\n"${cmd[@]}"\n`|Usar **array** evita problemas com espaços e caracteres especiais.|
|**Variável indireta (nome de variável em outra variável)**|`bash\nvar1="oi"\nref="var1"\neval echo \$$ref\n`|`bash\nvar1="oi"\nref="var1"\necho "${!ref}"\n`|`${!var}` funciona no Bash (não no sh puro).|
|**Criar variáveis numeradas em loop**|`bash\nfor i in 1 2 3; do\n eval var$i=$i\ndone\necho $var1 $var2 $var3\n`|`bash\nfor i in 1 2 3; do\n arr[$i]=$i\ndone\necho \"${arr[1]} ${arr[2]} ${arr[3]}\"\n`|Melhor usar **arrays** em vez de variáveis dinâmicas.|
|**Executar expressão montada dinamicamente**|`bash\nop="*"\neval echo 2 $op 3\n`|`bash\nop="*"\necho $(( 2 $op 3 ))\n`|Para aritmética, use **$(( ))** em vez de `eval`.|
|**Definir função dinamicamente**|`bash\nfname="ola"\neval \"$fname() { echo 'Oi'; }\"\nola\n`|```bash\nfname="ola"\ndeclare -f $fname > /dev/null||

<br>
### Regras de ouro

1. **Evite `eval` sempre que possível** → use arrays, `${!var}`, substituição aritmética.
2. **Nunca use com entrada de usuário** sem validação.
3. Use apenas quando realmente precisa de **expansão dupla** (avaliar uma string como código).