
As **permissões no Linux** controlam _quem pode fazer o quê_ com arquivos e diretórios. Elas são fundamentais para segurança e organização do sistema.

# **SUMÁRIO**

## [[#^d0d8a6| Inicio de tudo `ls`]]
## [[#^ca4ffd| Significado retorno `ls`]]

## [[#^affc5d| O que " x " pode corresponder (especial)]]

## [[#^3e025f|Diferença entre " x " e  " r " em diretórios]]

## [[#^aa5642| Regra de ouro ( " x " em diretórios)]]

## [[#^016917|Tabela de valores]]

## [[#^c1f256|Calcúlos dos valores "octal" para resultado "literal"]]

## [[#^1ee69f|Dicas para memorizar]]

## [[#^f5ef93|Utilizando "chmod" com valores octal]]


---
---

## **`ls -l`** ( onde tudo começa! )

^d0d8a6

- `ls -l` 
**ou**
- `ll` (alias)

**return:**   _drwxrwxr-x  5 britos britos 4.0K Mar 30 16:28  file.txt_


### **Significado:**

^ca4ffd

**`d`** -> Tipo diretório
**`rwx`** -> Permissão dono
**`rwx`** -> Permissão grupo
**`r-x`** -> Permissão outros
**`5`** -> Números de links 
- Para diretórios significa números de SUB-DIRETÓRIOS:
	- " . " o próprio diretório
	- " .. " o diretório pai
	-  " • " sub-diretórios internos -> **3 sub-diretórios dentro** ( 5 - 2 = 3)"
**`britos`** -> user dono
**`britos`** -> grp dono 
**`4.0K`** -> tamanho do diretório ("metadados"), não o conteúdo real
**`Mar 30 16:28`** -> Data ultima modificação
**`myCodes`** -> Nome do diretório


> As permissões no Linux são dividas em 3 grupos (dono, grupo, outro) - e em cada grupo são dadas as permissões. 


### RWX

**R**    =   Read
**W**   =   Write
**X**    =   Execução

---

### Permissão **`" X "`** (especial)

^affc5d

A permissão **" x "**,   pode corresponder a diferentes tipos de  permissões ! Isso vai depender do tipo do arquivo a que se refere-se a permissão. 

#### **Exemplos:**

- **Diretórios:**
	- **" `X` "** corresponde a poder **entrar no diretório (cd) e acessar seu conteúdo.** 

- **Arquivos:**
	- **" x "** corresponde a poder **executar o arquivo como um programa**.


---

#### Diferença entre `r` e `x` em diretórios

^3e025f

| Permissão | O que permite           |
| --------- | ----------------------- |
| `r`       | listar arquivos (`ls`)  |
| `x`       | entrar (`cd`) e acessar |
| `w`       | criar/deletar arquivos  |
#### Regra de ouro

^aa5642

> Sem **`X`** no diretório, você fica “na porta olhando”, mas não entra.


#### Tabela de valores

^016917

Cada permissão tem um valor numérico:

| Letra         | Valor |
| ------------- | ----- |
| `r` (read)    | 4     |
| `w` (write)   | 2     |
| `x` (execute) | 1     |
| `-` (nenhum)  | 0     |

#### Explicando a conversão dos valores

^c1f256

#### **rwx rwx r-x**

**rwx** -> 7   ( 4 + 2 + 1 = **7** )
**rwx** -> 7   ( 4 + 2 + 1 = **7** )
**r-x** -> 5   ( 4 + 0 + 1 = **5** )


#### Dica prática (pra memorizar rápido)

^1ee69f

	
	- **`7`** = tudo (`rwx`)
	- **`6`** = leitura + escrita (`rw-`)
	- **`5`** = leitura + execução (`r-x`)
	- **`4`** = só leitura (`r--`)


#### Aplicando permissões
Alterando/ adicionando permissões a arquivo/ diretórios

 - ####  chmod 
	 O comando `chmod` (change mode) é usado para **alterar permissões de arquivos e diretórios no Linux**. Ele é essencial para controle de acesso no sistema.

**Para Arquivos:**

- **`chmod u+w file.txt`**   ->    adicionando permissão de escrita no arquivo "**file.txt**" 
- **`chmod u-r file.txt`**   ->    excluindo permissão de leitura no arquivo  "**file.txt**" 
- **`chmod a=- file.txt`**   ->   definindo apenas permissão de leitura do arquivo "**file.txt**" para todos, tanto user, grupo e outros.

**Para diretórios:**

**`r`**   -> Permite **listar o conteúdo do diretório** (ver os nomes dos arquivos e pastas).

- **`chmod u+r test_folder`**

**`w`**   -> Permite **modificar o conteúdo do diretório**, ou seja: criar arquivos, deletar arquivos e  renomear arquivos

- **`chmod g-w testfolder 

> **Regra importante:**  O **" `w` "** sozinho não funciona direito!
> Para que ele seja útil, precisa estar junto com o **`x` (execução)**

**`x`**   -> Corresponde a poder **entrar no diretório (cd) e acessar seu conteúdo.** 

- **`chmod o=x testfolder`**


> 👉 **A exclusão de um arquivo NÃO depende da permissão do arquivo em si**, mas sim das permissões do **diretório onde ele está**.


---
---

# Permissões ACL

As **permissões avançadas no Linux** (**ACLs**) vão além do `rwx`. Elas controlam comportamentos especiais de arquivos e diretórios — muito usadas em servidores, como no seu cenário com Samba/AD.

## ACL - Access Control List

### Regras
Especificam com um usuário ou grupo acessa um arquivo.

Existem dois tipos de regras:

- **regras de acesso**
- **regras padrão**

> Importante !! Para que as **`ACL`** funcionem é necessário que as mesmas estejam instalas e habilitadas no sistema de arquivos ( **file system** ). 


### Ativar ACL na montagem de um `file system`

**Exemplo:**

	`mount -t ext4 -o acl /dev/sdb1 /mnt/files`


### Ativar ACL em partições montadas ( arquivo `/etc/fstab`)

	`vim /etc/fstab`
	`/dev/sdb1 / ext4 defaults,acl 0 1`


### Verificação do `file system` ACL ativo

	`tunefs2 -l /dev/sda1 | grep -i "default mount"

_**return esperado:  `user_xattr acl`**_


### **Verificação do KERNEL, suporte a _ACL_ e _Atributos estendidos_.** 

-  **Check - atributos estendidos do `fileSystem` (ativos ou não)**
		`egrep -i "ext4_fs_security" /boot/config-$(uname -r)

> **return:** " CONFIG_EXT4_FS_SECURITY=y"  _y ou n corresponde a compilação OK para atributos estendidos_

- **Check -  ACL do `fileSystem` (ativos ou não)**
	`egrep -i "ext4_fs_posix_acl" /boot/config-$(uname -r)

> **return:** " CONFIG_EXT4_POSIX_ACL=y"  _y ou n corresponde a compilação OK para ACL_


_OBS:. Caso o retorno dos comando fossem diferentes, teria que ser executada a instalação de um novo KERNEL com suporte a **atributos estendidos e ACL



# Permissões ( `chmod` )

O `chmod` é um comando do Linux usado para **alterar permissões de arquivos e diretórios**.

**Sintaxe:**

	`chmod [options] [mode] files`


> Existem 02 modos para se trabalhar com **permissões**:

- **Modo Octal**

| **x** | **w** | **r** |           **permissão**            | **valor octal** |
| :---: | :---: | :---: | :--------------------------------: | :-------------: |
|   0   |   0   |   0   |      sem permissão          .      |        0        |
|   0   |   0   |   1   |    execução                   .    |        1        |
|   0   |   1   |   0   |   gravação                    .    |        2        |
|   0   |   1   |   1   |        gravação e execução         |        3        |
|   1   |   0   |   0   | leitura                          . |        4        |
|   1   |   0   |   1   |     leitura e execução      .      |        5        |
|   1   |   1   |   0   |     leitura e gravação      .      |        6        |
|   1   |   1   |   1   |     permissão total          .     |        7        |

- **Modo literal**

**Categoria de usuários**

- **Símbolos:**
	- **`u`** ->  significa  "**usuários**", proprietário (dono) do arquivo ou diretório.
	- **`g`** ->  significa  "**grupo**", membro do grupo a qual o arquivo ou diretório pertence.
	- **`o`** ->  significa  "**outros**", todos os outros usuários do sistema que não são o proprietário nem membro do grupo.
	- **`a`** ->  significa  "**todos**", todas categorias de uma vez ( **ugo** ).

- **Ações:**

 **`+`** -> significa "**adicionar**", permissão especificada 

> "Exemplo:  **`u+x`**
> **Adiciona** permissão de execução (**`x`**) para o usuário (**`u`**).


**`-`** -> significa "**remover**", permissão especificada

> "Exemplo:  **`o-w`**
> **Remove** permissão de escrita (**`w`**) para outros (**`o`**).


**`=`** -> significa "**definir**", permissão exatamente como especificado, removendo qualquer outro tipo de permissão não listadas.

> "Exemplo:  **`ug=rw`**
> **Define** permissão de leitura (**`r`**) e escrita (**`w`**) para o usuário (**`u`**) e grupo ( **`g`** ), removendo o restante pré configurado.


## Aplicando as permissões ( `chmod `)

^f5ef93

Para entendimento de permissões no Linux, temos que entender sobre os usuários e grupos no Linux.
### [[Usuarios_Linux]]


