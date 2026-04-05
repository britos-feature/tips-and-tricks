## _**Path** (caminhos)_

Quando nos referimos a **`paths`**, estamos nos referindo à forma como representamos a localização de arquivos e diretórios no sistema de arquivos.

Existem basicamente **dois tipos principais** de caminhos: **absolute**, **relative**

- **Absolute**
  - Mostra a localização **completa** a partir da raiz do sistema (`/`).
  - Sempre começa com `/`.
  - Não depende de onde você está no momento (no diretório atual).

```sh
/etc/passwd
/home/usuario/documentos/arquivo.txt
/usr/bin/python3
```

> Funciona em qualquer lugar ao digitar caminho **absolute** > <br>

- **Relative**
  - Mostra a localização **a partir do diretório atual**.
  - **Não começa com `/`**.
  - Depende do diretório em que você está (`pwd` mostra o atual).

```sh
documentos/arquivo.txt
../imagens/foto.png
./script.sh
```

<br>
**Resumo:**

- **Absoluto** → começa em `/`, sempre o caminho completo.
- **Relativo** → depende do diretório atual, usa `.` e `..`.
- **Com `~`** → atalho para a home do usuário.
  <br>

---

## **\*Entendimento - Condição e Expressão**

Uma **_CONDIÇÃO_** é uma expressão que retorna um valor lógico: **verdadeiro (0)** ou **falso (≠0)**.  
 Essas condições são usadas, por exemplo, em estruturas como `while`, `if`, ou no próprio `[ ]` (test)

Uma **_EXPRESSÃO_** é qualquer construção que combina **valores, variáveis, operadores e comandos** para produzir um resultado (que pode ser um número, string ou valor lógico).

**Resumindo!**

- **Condição** = expressão que retorna um valor lógico (verdadeiro/falso).
- **Expressão** = qualquer cálculo, teste ou comando que gera um resultado.

### **_Quadro de operadores_**

- **_Arquivos e diretórios_**

| Operador | Significado                   |
| -------- | ----------------------------- |
| `-e`     | Existe (arquivo ou diretório) |
| `-f`     | Existe e é arquivo comum      |
| `-d`     | Existe e é diretório          |
| `-r`     | É legível                     |
| `-w`     | É gravável                    |
| `-x`     | É executável                  |
| `-s`     | Existe e não está vazio       |

<br>
-  **_Strings_**

| Operador       | Significado                              |
| -------------- | ---------------------------------------- |
| `-z "$str"`    | Verdadeiro se a string for **vazia**     |
| `-n "$str"`    | Verdadeiro se a string **não** for vazia |
| `"$a" = "$b"`  | Igualdade                                |
| `"$a" != "$b"` | Diferente                                |

<br>
- **_Numbers_**

| Operador | Significado                           |
| -------- | ------------------------------------- |
| `-eq`    | Igual **(equal)**                     |
| `-ne`    | Diferente **(different)**             |
| `-lt`    | Menor que **(less than)**             |
| `-le`    | Menor ou igual **(less or equal)**    |
| `-gt`    | Maior que **(greater than)**          |
| `-ge`    | Maior ou igual **(greater or equal)** |

<br>
- **_Lógicos_**

| Operador | Significado   |
| -------- | ------------- |
| `&&`     | E (AND)       |
| \|\|     | OU (OR)       |
| `!`      | Negação (NOT) |

<br>

---

---

## **\*( ) parênteses**

Execução de comandos em **Subshell**(processo filho).
Significa que <b><i>variáveis, diretórios ou alterações feitas dentro do Subshell, não afetam o Shell principal</i></b>\*

**_Exemplo:_**

```sh

(cd /tmp && ls) # exec in Subshell
pwd # Shell atual

```

<br>
- ***Executando vários comandos em sequência <u>Subshell</u>***

```sh

(name="Alex"; lasname="Brito"; echo "$name $lastname") # return "Alex Brito", exec in Subshell.

echo "$name $lastname" # exec Shell atual
# Obs: return empty(vazio) - pois variáveis foram criadas em um Subshell.

```

<br>
- ***Rodar Subshell em background*** `&`
_Se combinar `()` com `&` , o subshell roda em background os comandos_

```sh

(sleep 5; echo "Success!") &
echo "Enquanto isso, sigo codando!..."

```

<br>
- ***Subshell (expressão ariméticas)***
_Se você usar `(( ))`, vira **aritmética** no shell_

```sh

echo $(( 2 + 3 )) # return 5

```

#### **_Resumindo_**

- `(comando)` → roda em **subshell**.
- `(cmd1; cmd2; ...)` → agrupa comandos isoladamente.
- `(comando) &` → roda em background.
- `$((expressão))` → faz cálculos aritméticos.
  <br>
  > **\*OBS:. Por padrão DEFAULT**, um subshell **executa os comandos e depois termina automaticamente\***.
  > <br>

---

---

## **_Variable_**

_No **shell (bash, zsh, sh, etc.)**, variáveis podem existir apenas no **shell atual** ou podem ser **exportadas** para ficarem disponíveis também em processos filhos (subshells, scripts, etc.)._

- **\*Default,** variável criadas apenas pertence ao scope **ambiente local ( "environment" )\***
- _Variável **exportada(export)** irá pertence ao scope **ambiente local e subshell ( filho do shell atual )**_
- \_**`env`** comando para listar variáveis de **ambiente local e exportadas** e comando **`export -p`** só variáveis exportadas
-

### **_Formas de criar e exportar variables_**

- _Definindo e exportando variables_

```sh

# modo 1
name="Alex"
export name

# modo 2 (diretamente)
export name="Alex"

```

>     _**" `export` "** modos que exportam as variáveis, para utilização  no **ambiente local (environment)** and **Subshells**._
>
> <br>

- _Definindo e exportando **variables fixas** (persistente no sistema)_
  - _Necessário adicionar o **`export`** no arquivo de configuração que é carregado em cada login._

**_Para um único usuário_**

```sh

# ~/.bashrc → → → carregado a cada abertura de terminal interativo.
# ~/.bash_profile ou ~/.profile → → → carregados no login.
nano ~/.bashrc
export MY_VAR="value" # adicionar no final do arquivo de configuração

source ~/.bashrc # comando que recarrega arquivo com as novas configurações

# Agora, sempre que abrir um terminal, a variável estará disponível

```

>     _**`source`** O comando do shell **(bash, zsh, etc.),** executa arquivos de script **no mesmo shell atual**, em vez de abrir um subshell.
>     Variáveis, funções e aliases definidos no script **permanecem ativos**.
>
> <br>

\*\*\*Para um todos usuário (all)

```sh

# /etc/environment → → → carregado na abertura da sessão
sudo nano /etc/environment
MY_VAR="value"

# Salve e reinicie a sessão (ou o sistema) para aplicar.

```

<br>

---

---

## **_Partições_**

- **_Listar partições_** _(listar partições existente no disco)_
	`lsblk` <small> or </small> `fdisk -l`

- _**Lista tipo`fileSytem`, espaço em disco**_ _(*partições montadas, Mostra o espaço total, utilizado e disponível.)_
	`df -h`

- _**Lista diretórios, sub-diretórios e arquivos**_ _(Mostra o tamanho ocupado por um caminho específico)_
	`du -sh`

- _**Montar partições**_ (tornar o conteúdo do disco acessível dentro do sistema de arquivos)
	`mount /dev/sda2 /local_montagem` <small> or </small> `mount -t ntfs-3g /dev/sda2 /local_montagem`

>     o flag `-t` corresponde ao ***file system***, sistema de arquivos que será utilizado para montagem da partição.


## **_Procedimento básico para adicionar  novo 'HDs'**_
_**(sistemas de blocos)**_

O processo básico é:

1. Identificar o disco
2. Criar partição
3. Formatar
4. Criar ponto de montagem
5. Montar o disco
6. Configurar montagem automática (`fstab`)

```bash
# 1️- Identificar o novo HD
lsblk
# or
sudo fdisk -l

# 2 - Criar partição no disco
fdisk /dev/sdb

# Dentro do menu utilizar-se de:
# n -> nova partição
# p -> primary
# 1 -> numero partição
# enter / enter
# w -> salvar 

# 3 - Formatar o disco
sudo mkfs.ext4 /dev/sdb1

# 4 - Criar ponto de montagem
sudo mkdir /mnt/files

# 5 - Montar o disco
sudo mount /dev/sdb1 /mnt/files # manualmente
# verificação
df -h
# or
lsblk


# 6 - Configurar montagem automática (fstab)
# Descobrir o numero do UUID do disco
# exemplo : /dev/sdb1: UUID="6f8c2b1a-9f34-4c6e-bb0f-7c9e5c4d2a11" TYPE="ext4"
blkid 

# Editar o fstab ('/etc/fstab')
sudo nano /etc/fstab
# Adicionar ao final da linha (UUID)
UUID=6f8c2b1a-9f34-4c6e-bb0f-7c9e5c4d2a11 /mnt/arquivos ext4 defaults 0 2

# Testar 'fstab' (IMPORTANTE) - antes de reiniciar
sudo mount -a
```



---

---
ualizar informações do sistema operacional no Linux_

**_Informações Gerais do Sistema Operacional_**
`cat /etc/os-release` <small>or</small> `lsb_release -a`

>     `lsb_release` -  pode precisar ser instalado em algumas distros  ! `sudo apt install lsb-release`

**_Resumo do Sistema (completo)_**
`neofetch`

**_Nome e Versão do Kernel_**
`uname -a` <small>or</small> `uname -r`

**_Arquitetura e Processador_**
`lscpu`

**_Memória RAM_**
`free -h`

**_Espaço em disco_**
`df -h`

**_Informações Detalhadas do Hardware (resumo)_**
`inxi -Fxz`

**_Informações detalhadas do sistema_**
`hostnamectl`

>     _(especialmente útil em sistemas com **systemd**, como Ubuntu moderno, Fedora, Arch, etc.)

**_Monitoramento de processos em tempo real_** -> **_[[Linux/Command_top|Command " top "]]_**
`top`

---

---

## **_Script_** (criação de scripts)

**Criação de script**
 - Criar um arquivo com a extensão `.sh`
 - Primeira linha do arquivo deve conter o caminho da execução  **ex:** `#!/bin/bash`
 - Permissão para execução `chmod +x script.sh`

**Execução do script**
`./script.sh`
`bash script.sh` ``
`sh script.sh`

**_Tornar script acessível de qualquer lugar (PATH)._**
`sudo mv sripct.sh /usr/local/bin/`

---

---

## **_Empacotar/comprimir arquivos_**

\*Saiba mais em **[[Linux/Command_tar.gz]]\***

```sh
tar -cvzf myFile.tar.gz # cria empacota/comprime
tar -xvzf myFile.tar.gz # extrair descomprime/descompacta

# utilize z (gzip)
# utilize j (bzip2)
# utilize J (xz)
```

<br>

---

---

## **_Baixar vídeos do youtube_** (utilitário `yt-dlp`)

Esse é o método mais prático e seguro é usar o **`yt-dlp`**, um utilitário de linha de comando moderno (atualização do antigo **`youtube-dl`**).

- **Instalar o `yt-dlp`**

```sh
sudo apt update
sudo apt install yt-dlp -y
```

<br>
- **Baixar um vídeo do YouTube**

```sh
yt-dlp "https://www.youtube.com/watch?v=EXEMPLO"
```

<br>
- **Baixar apenas o áudio (MP3 ou outro formato)**

```sh
yt-dlp -x --audio-format mp3 "https://www.youtube.com/watch?v=EXEMPLO"
```

<br>
- **Escolher qualidade ou formato específicos**

```sh
# Mostrar todas as opções de qualidade
yt-dlp -F "https://www.youtube.com/watch?v=EXEMPLO"

# escolhendo (exemplo 22)
yt-dlp -f 22 "https://www.youtube.com/watch?v=EXEMPLO"
```

<br>
- **Salvar em uma pasta específica**

```sh
yt-dlp -o "~/Vídeos/%(title)s.%(ext)s" "https://www.youtube.com/watch?v=EXEMPLO"
```

<br>
- **Atualização via APT

```sh
sudo apt update
sudo apt install yt-dlp -y

# Isso vai instalar a **última versão disponível nos repositórios oficiais** (ou do _backports_, se habilitado).
```

<br>
- **Habilitar o backports, caso esteja desabilitado**

```sh
sudo add-apt-repository ppa:tomtomtom/yt-dlp
sudo apt update
sudo apt install yt-dlp -y
```

<br>
- #### **Remover o pacote do APT e instalar via PIP (mais atualizado)**
	- Se quiser sempre a versão mais nova (a do site oficial)

```sh
# remove do APT
sudo apt remove yt-dlp

# install via PIP (python)
pip install -U yt-dlp

# verificar version
yt-dlp --version
```

<br>
- **Install manual**

```sh
sudo wget https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -O /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp
```

<br>

---

## _Apagar completament um Pendrive

`sudo dd if=/dev/zero of=/dev/sdX bs=4M status=progress`

**Segurança máxima.**
`sudo shred -v -n 3 /dev/sdX` --> X = partição

---
ls
## _Criar pendrive bootável_

### Método 1

- **GNOME Disks (Discos do Linux)**

_(vem instalado no Ubuntu, Mint e derivados)_

**Passo 1 — Abra o programa**  
Pesquisar no menu: **"Discos"** ou **"GNOME Disks"**

**Passo 2 — Selecione o pendrive**

**Passo 3 — Clique em:**

> ☰ (ícone de menu) → **Restaurar imagem de disco…**

**Passo 4 — Escolha a ISO** e confirme.

Este método é muito confiável para distros Linux.

### Método 2

- **Pelo terminal (dd)**
  \_Cuidado: esse comando pode apagar o HD se usar o disco errado.

> **Descobrir o pendrive:**

```bash
lsblk
```

**Exemplo:**

```
sda  (HD)
sdb  (pendrive)
```

> **Criar o pendrive bootável:**

```bash
sudo dd if=/caminho/da/imagem.iso of=/dev/sdX bs=4M status=progress oflag=sync
```


**Substitua:**

- `/caminho/da/imagem.iso`
- `sdX` → exemplo: `sdb` (sem número, **NÃO** usar `sdb1`)

---

Commando a pesquisar
shopt
find
grep
sed
awk
