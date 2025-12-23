# ***SSH Advanced***

Usar **SSH** para fazer _upload_ e _download_ de arquivos é algo muito comum em servidores Linux. Existem algimas fomas e ferramentas que funcionam sobre o protocolo SSH para isso.

## Usando `scp` (Secure Copy)

- ***Upload (do seu computador → servidor)***

```shell
# file upload
scp /caminho/local/arquivo.txt usuario@servidor:/caminho/remoto/

# folder upload
scp -r /caminho/local/arquivo.txt usuario@servidor:/caminho/remoto/
```

- ***Download (do servidor → seu computador)***

```shell
# file download
scp usuario@servidor:/caminho/remoto/arquivo.txt /caminho/local/

# folder download
scp -r usuario@servidor:/caminho/remoto/arquivo.txt /caminho/local/
```

***Example:***

```shell
scp meu_arquivo.txt root@192.168.0.10:/var/www/
scp root@192.168.0.10:/var/www/index.html ~/Downloads/
```

> Simples, já vem em sistemas Linux e Mac. No Windows você pode usar via Git Bash ou com WSL.


---

## Usando `rsync` (sincronização eficiente)

***`rsync`*** é muito usado para transferir arquivos ou sincronizar pastas inteiras via SSH, de forma otimizada.

- ***Uploads***

```shell
rsync -avz /caminho/local/ usuario@servidor:/caminho/remoto/
```

- ***Downloads***

```shell
rsync -avz usuario@servidor:/caminho/remoto/ /caminho/local/
```

***Example:***

```shell
rsync -avz ./site/ root@192.168.0.10:/var/www/html/
```

> Muito bom para pastas grandes, pois só transfere diferenças.


---


## Usando SFTP (SSH File Transfer Protocol)

Modo interativo:

```shell
sftp usuario@servidor
```

***Depois você tem um prompt tipo FTP:***

```shell
put arquivo.txt          # Upload
get arquivo.txt          # Download
cd /pasta/remota/        # Navegar no servidor
lcd /pasta/local/        # Navegar no seu computador
ls                       # Listar arquivos
```

> Ótimo para quem prefere um modo mais manual e interativo.


## Resumo:

| Ferramenta | Melhor uso                                          |
| ---------- | --------------------------------------------------- |
| `scp`      | Cópia simples de arquivos ou pastas                 |
| `rsync`    | Sincronização de pastas e transferência eficiente   |
| `sftp`     | Modo interativo para gerenciar arquivos remotamente |

