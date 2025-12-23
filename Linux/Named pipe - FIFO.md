Um **named pipe** (também chamado de **FIFO**) no shell é um tipo especial de arquivo usado para comunicação entre processos. Ele funciona como um **canal de comunicação unidirecional**: um processo escreve dados no pipe, e outro processo lê esses dados, **sem necessidade de arquivos temporários no disco**.

**Diferença entre pipe "`|`" e named pipe "`mkfifo`"

- O **pipe comum** (`|`) conecta **diretamente** a saída de um processo com a entrada de outro, mas só existe **durante a execução** da linha de comando.
	`ls | grep txt`<br>
-  O **named pipe (FIFO)** é um **arquivo especial no sistema de arquivos**, criado com `mkfifo`.  
Ele **persiste até ser removido**, e qualquer processo pode abrir esse "arquivo" para escrever ou ler.
	`mkfifo my_fifo`
<br>

**Exemplo basic**

```sh
# terminal 1

echo "Secret msg" > my_fifo
# esse comando vai bloquear até que alguém esteja lendo o processo

```

```sh
# terminal 2

cat < my_fifo
# saída: "Secret msg"
# agora processo consumidor lê a mensagem

```
<br>
**Exemplo prático**

```sh
# Comunicação entre processos diferentes

# Terminal 1: produtor
mkfifo canal
dmesg > canal

# Terminal 2: consumidor
grep usb < canal

```
<br>

**OBS. IMPORTANTES !

- Um FIFO **não armazena dados permanentemente**. Ele só segura os dados **até serem lidos**.
- Se ninguém estiver lendo, o processo escritor **fica bloqueado** (e vice-versa).
- Funciona **como um pipe**, mas com a vantagem de permitir a conexão entre processos que não foram iniciados juntos na mesma linha de comando.


### Example script

```sh

#!/bin/bash

# Criar o FIFO se não existir
FIFO="meu_fifo"
[ -p "$FIFO" ] || mkfifo "$FIFO"

# Produtor (gera mensagens a cada 2s)
{
  for i in {1..5}; do
    echo "Mensagem $i" > "$FIFO"
    sleep 2
  done
} &   # roda em background

# Consumidor (lê do FIFO)
while read linha < "$FIFO"; do
  echo "[Consumidor recebeu] $linha"
done

```
<br>
#### Como funciona

1. Criamos o arquivo especial com `mkfifo`.
2. O **produtor** escreve mensagens no FIFO.
3. O **consumidor** (o `while read`) lê cada mensagem assim que ela chega.
4. A comunicação acontece **sem arquivo temporário**, só pelo FIFO.


#### Saída esperada
Se você rodar esse script, verá algo como:

```csharp

[Consumidor recebeu] Mensagem 1
[Consumidor recebeu] Mensagem 2
[Consumidor recebeu] Mensagem 3
[Consumidor recebeu] Mensagem 4
[Consumidor recebeu] Mensagem 5

```
<br>
