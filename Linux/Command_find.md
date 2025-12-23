O **`find`** é um dos comandos mais poderosos do Linux/Unix para **procurar arquivos e diretórios** dentro de um sistema de arquivos. Ele não só encontra, mas também permite **executar ações** nesses arquivos.

[[#^9a7c9e|Lista de critérios mais utilizados (primários)]]
[[#^057a10|Lista de critério de pesquisas]]
[[#^7f7cf6|Lista de ações mais utilizadas]]
[[#^05d58e|Ordem de utilização dos critérios]]
[[#^a78b66|Guia rapido find]]


***Sintaxe:***
`find [dir_initial ...] criterios [ação]`

***Onde:***
- **`dir_initial`** = é o diretório onde se oiriginará a pesquisa
- **`criterios`*** = são os critérios de pesquisa que podem ser conectados por **`-o (OR)`** ou **-a (AND = default)**  
- **`ação`** = defini a ação que será tomada com os arquivos que atenderem aos **`criterios`**

---

#### ***Lista dos critérios mais usados (ou "primários") para pesquisa com `find`:***

^9a7c9e

- ***Critérios por nome e caminho (secundário)***

	- `-name PADRÃO` → Nome do arquivo (com curingas `*`, `?`, `[]`).
	- `-iname PADRÃO` → Igual ao `-name`, mas **ignora maiúsculas/minúsculas**.
	- `-path PADRÃO` → Caminho completo (incluindo diretórios).
	- `-ipath PADRÃO` → Versão do `-path` sem diferenciar maiúsculas/minúsculas.
	- `-regex REGEX` → Casar com expressão regular.
	- `-iregex REGEX` → Igual ao acima, ignorando maiúsculas/minúsculas.

> OBS IMPORTANTE SOBRE `-regex / -iregex`
> Usa `-regex / -iregex`**(expressão regular)** é para casar o **caminho completo** do arquivo, <span style="color: red">não apenas o nome.</span>

<br>

- ***Critérios por tipo de arquivo (primário)***

	- `-type f` → Arquivos comuns.
	- `-type d` → Diretórios.
	- `-type l` → Links simbólicos.
	- `-type c` → Dispositivo de caractere.
	- `-type b` → Dispositivo de bloco.
	- `-type p` → FIFO (pipe nomeado).
	- `-type s` → Socket.

<br>

- ***Critérios por tempo (terçario)***

	- `-atime N` → Último acesso há **N dias**.
	- `-mtime N` → Última modificação de conteúdo há **N dias**.
	- `-ctime N` → Última modificação de metadados (permissões, dono, etc.) há **N**
	- `-amin N`, `-mmin N`, `-cmin N` → Versões em **minutos**.
	- **Sinais úteis**:
		- `+N` → maior que N dias/minutos.
		- `-N` → menor que N dias/minutos.
		-   `N` → exatamente N dias/minutos.

<br>

- ***Critérios por tamanho***
	`-size N[cwbkMG]` → Tamanho do arquivo.
		- `c` → bytes.
		- `k` → kilobytes.
		- `M` → megabytes.
		- `G` → gigabytes.
		- `+N` maior que, `-N` menor que, `N` exatamente.
		- 
***Exemplo:*** `-size +100M` → maiores que 100 MB.

<br>

- ***Critérios por permissões e dono***

	- `-user USUÁRIO` → Pertence a um usuário.
	- `-group GRUPO` → Pertence a um grupo.
	- `-uid N` → Pertence ao UID N.
	- `-gid N` → Pertence ao GID N.
	- `-nouser` → Não possui dono válido.
	- `-nogroup` → Não possui grupo válido.
	- `-perm MODE` → Arquivos com permissões específicas.
		- `-perm 644` → exatamente 644.
		- `-perm -644` → deve ter **pelo menos** estas permissões.
		- `-perm /644` → deve ter **alguma destas** permissões.

<br>

- ***Critérios por links***

	- `-links N` → Arquivos com exatamente N hard links.
	- `-lname PADRÃO` → Nome do destino do link simbólico.
<br>

- ***Critérios por profundidade e caminho (global)***

	- `-maxdepth N` → Limita profundidade máxima da busca.
	- `-mindepth N` → Limita profundidade mínima.
	- `-mount` ou `-xdev` → Não atravessa para outros sistemas de arquivos.
<br>

---


#### ***Lista de critérios de pesquisas mais usados utilizando do `find`:***

^057a10

##### ***Exemplos:***

<b><code>-name PADRÃO</code></b> = *Nome do arquivo (sensível a maiúsc./minúsc.).*

	find . -name "*.txt"
<br>

<b><code>-iname PADRÃO</code></b>  = *Nome do arquivo (ignora maiúsc./minúsc.).*

	find . -iname "*.jpg"
<br>

<b><code>-path PADRÃO</code></b> = *Caminho completo casa com o padrão.*

	find /etc -path "*/ssh/*"
<br>

<b><code>-ipath PADRÃO</code></b> = *Igual ao `-path`, mas insensível a maiúsc./minúsc.*

	find /etc -ipath "*/SSH/*"
<br>

<b><code>-regex REGEX</code></b> = *Casa com expressão regular no caminho.*

	find . -regex ".*\.log"
<br>

<b><code>iregex REGEX</code></b> = *Igual ao anterior, ignorando maiúsc./minúsc.*

	find . -iregex ".*\.(jpg)"
<br>

<b><code>-type f/d/l/...</code></b> = *Tipo de arquivo (`f`=arquivo, `d`=diretório, `l`=link, etc.).*

	find . -type d
<br>

<b><code>-size N[cwkMG]</code></b> = *Arquivos de tamanho **N** (bytes, KB, MB, GB). `+N` = maior, `-N` = menor.*

	find . -size +100M
<br>

<b><code>-user USUÁRIO</code></b> = *Arquivos pertencentes ao usuário.*

	find /home -user joao
<br>

<b><code>-group GRUPO</code></b> = *Arquivos pertencentes ao grupo.*

	find /var -group www-data
<br>

<b><code>-uid N</code></b> = *UID específico.*

	find . -uid 1000
<br>

<b><code>-gid N</code></b> = *GID específico.*

	find . -gid 33
<br>

<b><code>-nouser</code></b> = *Arquivos sem donos sem válidos*

	find / -nouser
<br>

<b><code>-nogroup</code></b> = *Arquivos sem grupo válido.*

	find / -nogroup
<br>

<b><code>-perm MODE</code></b> = *Permissões específicas (`644`, `-644`, `/644`). *

	find . -perm 755
<br>

<b><code>-links N</code></b> = *Arquivos com exatamente N hardlinks.*

	find / -links 2
<br>

<b><code>-lname PADRÃO</code></b> = *Links simbólicos cujo destino casa com o padrão.*

	find . -type l -lname ".so"
<br>

<b><code>-atime N</code></b> = *Último acesso há N dias (`+N` >, `-N` <).*

	find . -atime -7
<br>

<b><code>-mtime N</code></b> = *Última modificação de conteúdo há N dias*

	find . -mtime +30
<br>

<b><code>-ctime N</code></b> = *Última modificação de metadados há N dias.*

	find /etc -ctime -1
<br>

<b><code>-amin N / -mmin N / -cmin N</code></b> = *Versões em minutos de `atime`, `mtime`, `ctime`.*

	find . -mmin -10
<br>

<b><code>-depth</code></b> = *Processa o conteúdo de cada diretório antes do diretório em si.*

	find ~/Document -depth -name "*.txt"
<br>

<b><code>-maxdepth N</code></b> =  *Limita profundidade máxima da busca.*

	find . -maxdepth 1 -name "*.sh"
<br>

<b><code>-mindepth N</code></b> = *Limita profundidade mínima da busca*

	find . -mindepth 2 -type f
<br>

<b><code>-mount/ -xdev</code></b> = *Não atravessa para outros sistemas de arquivos.*

O `-mount` (ou `-xdev`, que é sinônimo) serve para **impedir que o `find` atravesse outros sistemas de arquivos**. 

	find / -xdev -name "*.log"

> Em um ponto de montagem dentro do diretório que está pesquisando (por exemplo, um HD externo, uma partição ou até `/proc`, `/sys`, etc.), o `find` **não vai entrar nele**.
<br>

---

#### ***Lista das ações mais usados utilizando o com `find`:***

^7f7cf6

##### ***Exemplos:***

<b><code>-print</code></b> = *Exibe o caminho completo de cada arquivo encontrado (padrão).*

	find /etc -name "*.conf" -print
<br>

<b><code>-print0</code></b> = *Igual ao `-print`, mas separa por caractere NUL (`\0`), útil com `xargs -0`.*

	find . -type f -
<br>

<b><code>-exec cmd {} \</code></b> = *Executa o comando para **cada arquivo encontrado** (`{}` = arquivo).*

	find . -name "*.log" -exec rm {} \
<br>

<b><code>-exec cmd {} +</code></b> = *Executa o comando passando **vários arquivos de uma vez** (mais eficiente).*

	find . -name "*.txt" -exec cat {} +
<br>

<b><code>-ok cmd {} \</code></b> = Igual ao `-exec`, mas pede **confirmação interativa**.

	find . -type f -ok rm {} \;
<br>

<b><code>-delete</code></b> = Remove os arquivos/diretórios encontrados.

	find /tmp -type f -name "*.tmp" -delete
<br>

<b><code>-ls</code></b> = *Mostra detalhes no estilo `ls -dils`.*

	find . -type f -ls
<br>

<b><code>-fls arquivo</code></b> = *Igual ao `-ls`, mas grava em um arquivo.*

	find /var/log -type f -fls lista.txt
<br>

<b><code>-printf FORMAT</code></b> = *Exibe saída personalizada sobre os arquivos.*

	find . -type f -printf "%p %s bytes\n"
<br>

<b><code>-fprintf arq FORMAT</code></b> = *Igual ao `-printf`, mas grava em arquivo.*

	find . -type f -fprintf saida.txt "%p %u %g\n"
<br>

<b><code>-quit</code></b> = *Encerra a busca imediatamente após o primeiro resultado.*

	find / -name passwd -print -quit
<br>

---

#### ***Ordem lógica para os critérios***

^05d58e

O `find` tem uma **ordem lógica** para os critérios, mas o mais importante é entender que ele funciona como um **filtro sequencial**.

***All, Sintaxe:***

`find [path] [options] [test] [actions]`


- #### ***Path (caminho)***
É **onde** indicamos onde o `find` vai iniciar sua procurar <span style="color:red">(se não passar nada, ele assume `.` = diretório atual)</span>

```sh
find /home/Documents 
# /home/Documents é o path = caminho
```
<br>

- #### ***Options (global)***
Afetam a forma de busca (elas sempre vem primeiro de tudo, depois do "path")

- `-maxdepth N` → até N níveis de subdiretórios
- `-mindepth N` → a partir do nível N
- `-mount` → não atravessa outros sistemas de arquivos

```sh
find . -maxdepth 2 -xdev -type f -name "*.txt"
```


- #### ***Testes (critérios de seleção)
Filtros que dizem **quais arquivos** entram no resultado:

- `-type f` → apenas arquivos normais
- `-name "*.txt"` → nome igual ao padrão
- `-size +1M` → maior que 1 MB
- `-mtime -7` → modificados nos últimos 7 dias

Esses testes podem ser combinados com operadores lógicos:

- `-a` (AND – implícito se não colocar nada)
- `-o` (OR)
- `!` ou `-not` (negação)

```sh
find . -type f -name "*.txt" -size +1M 
# todos arquivo comuns com extensão .txt, maiores que 1 Mega

find . \( ! -type d -o -size +1M \) -iname "*codes"
# todos arquivo que seja um diretório ou arquivo que não sejam maiores que 1 Mega, que tenham o nome terminado com "codes/ CODES"
```

#### ***### **Ações**
O que fazer com os arquivos encontrados:

- `-print` → mostrar (padrão se nenhuma ação for passada)
- `-exec comando {} \;` → executar comando em cada arquivo
- `-delete` → apagar arquivos encontrados

```sh
find . -type f -name "*.log" -delete
```


***RESUMO da lógica***

```sh
find . -maxdepth 2 -type f -name "*.txt" -size +1M -print
```

Fluxo:

1. Começa em `.` (diretório atual). -> **path**
2. Só desce **até 2 níveis** (`-maxdepth 2`). -> **options globais**
3. Pega um item → testa se é arquivo comum (`-type f`) -> **test filter**
4. Se sim → testa se o nome termina em `.txt` (`-name "*.txt"`). -> **test name**
5. Se sim → testa se tem mais de 1 MB (`-size +1M`). -> **test size**
6. Se todos forem verdadeiros → **executa ação** (`-print`). -> **actions**

---

# Guia rápido do `find`

^a78b66

## 🔎 Critérios de busca

| Opção             | Descrição                                     | Exemplo                         |
| ----------------- | --------------------------------------------- | ------------------------------- |
| `-name "padrão"`  | Busca por nome (case sensitive)               | `find . -name "arquivo.txt"`    |
| `-iname "padrão"` | Busca por nome (ignora maiúsculas/minúsculas) | `find . -iname "*.jpg"`         |
| `-type f`         | Apenas arquivos                               | `find . -type f -name "*.sh"`   |
| `-type d`         | Apenas diretórios                             | `find . -type d -name "backup"` |
| `-size +N`        | Arquivos maiores que N (k, M, G)              | `find . -size +100M`            |
| `-size -N`        | Arquivos menores que N                        | `find . -size -10k`             |
| `-mtime -N`       | Modificados nos últimos N dias                | `find . -mtime -7`              |
| `-atime -N`       | Acessados nos últimos N dias                  | `find . -atime -1`              |
| `-perm 644`       | Permissões exatas                             | `find . -perm 755`              |
| `-user nome`      | Arquivos de um usuário                        | `find /var -user root`          |
| `-group nome`     | Arquivos de um grupo                          | `find /var -group admin`        |

## ⚡ Ações úteis

|Opção|Descrição|Exemplo|
|---|---|---|
|`-print`|Mostra arquivos (padrão)|`find . -name "*.txt" -print`|
|`-exec comando {} \;`|Executa um comando para cada arquivo|`find . -name "*.log" -exec rm {} \;`|
|`-exec comando {} +`|Executa comando em lote (mais rápido)|`find . -name "*.log" -exec rm {} +`|
|`-ok comando {} \;`|Como `-exec`, mas pede confirmação|`find . -name "*.sh" -ok chmod +x {} \;`|
|`-delete`|Remove arquivos direto|`find . -name "*.tmp" -delete`|

## 🛠️ Exemplos práticos

1. **Apagar arquivos temporários**
`find /tmp -type f -name "*.tmp" -delete`

2. **Listar arquivos grandes**
`find / -type f -size +1G 2>/dev/null`

3. **Encontrar arquivos sem permissão de leitura**
`find . -type f ! -readable`

4. **Procurar múltiplos tipos de arquivo**
`find . \( -name "*.txt" -o -name "*.md" \)`

5. **Contar arquivos de log**
`find /var/log -type f -name "*.log" | wc -l`
