---
---
## **DAEMONS** (entendimento)
	**Daemons** são **programas que rodam em segundo plano** no Linux, sem interação direta com o usuário.

	👉 Em uma frase:  
		**Daemon = serviço que fica rodando “por trás”, esperando para executar tarefas.**


**smdb** -> Responsável pelo compartilhamento de arquivos e permissões
	- 📁 Compartilha pastas e arquivos
	- 🔐 Controla autenticação (usuário/senha)
	- ✏️ Permite leitura e escrita
	- 🔒 Aplica permissões
	- 
>💡 **Sem o `smbd`, o Samba praticamente não funciona** 

**nmbd** -> **Responsável por nomes NetBIOS (descoberta na rede)**
	- 📡 Anuncia o servidor na rede
	- 🔎 Resolve nomes (tipo: `\\SERVIDOR`)
	- 🧭 Ajuda máquinas Windows a encontrarem o Samba

>💡 Hoje em redes modernas, ele é menos usado (DNS substitui muita coisa)


**winbindd** -> **Integra Linux com domínio Windows (Active Directory)**
	- 👥 Permite login com usuários do domínio
	- 🔗 Integra com Active Directory
    - 🆔 Mapeia usuários Windows para usuários Linux

>💡 Usado em ambientes corporativos


**smbd-notifyd** ->  (menos conhecido) / **Notifica mudanças de arquivos**
	- 🔔 Informa quando arquivos são alterados
	- 🔄 Atualiza clientes conectado

### Configuração de INICIALIZAÇÃO SAMBA durante o `boot` (unit)

**unit** ->  Uma **unit (unidade de serviço)** é **qualquer recurso que o systemd gerencia**. No Linux moderno (Ubuntu) por exemplo, o sistema usa o **systemd**.

#### Exemplo de tipos comuns de units

| Tipo      | Exemplo             | Função                  |
| --------- | ------------------- | ----------------------- |
| `service` | `smbd.service`      | Serviços (daemons)      |
| `mount`   | `/mnt/arquivos`     | Montagem de disco       |
| `socket`  | `smbd.socket`       | Comunicação de rede     |
| `target`  | `multi-user.target` | Agrupamento de serviços |

#### Exemplo com Samba

- `smbd.service` → controla o daemon `smbd`
- `nmbd.service` → controla o `nmbd`


### Configuração - (personalizando a`unit` do `samba`)

> O **código fonte do samba** já vem como uma **unit**, basta copia-la, personaliza-la e configura-la conforme necessidade.

#### Localizando e copiando a `unit` do samba (source code)

- **Comando:**
	`find /usr/local/samba-4.23.2/ -type f -iname "samba.service" -exec cp -v {} /etc/systemd/system/samba-ad-dc.service \;`

> Esse comando faz o seguinte:
>  Procura um arquivo chamado `samba.service` dentro de `/usr/local/samba-4.23.2/` e copia ele para `/etc/systemd/system/` com outro nome (`samba-ad-dc.service`).

### Quebrando em partes o comando para entendimento

 - 1. **`find /usr/local/samba-4.23.2/`**

	- `find` → comando usado para **procurar arquivos e diretórios**
	- `/usr/local/samba-4.23.2/` → diretório onde a busca começa

📌 Ou seja:
> "Procure dentro dessa pasta e tudo que está dentro dela"


- 2. **`-type f`**

	- Filtra apenas **arquivos**
	- Ignora diretórios, links, etc.

📌 Resultado:
> Só arquivos normais serão considerados


- 3. **`-iname "samba.service"`**

	- `-name` → busca por nome exato
	- `-iname` → **ignora maiúsculas/minúsculas** (case insensitive)

📌 Exemplo que ele encontraria:
	- samba.service ✅
	- Samba.Service ✅
	- SAMBA.SERVICE ✅


- 4. **`-exec ... \;`**
	Aqui está a parte mais poderosa 👇

	- `-exec` → executa um comando para cada resultado encontrado
	- `{}` → representa o arquivo encontrado
    - `\;` → finaliza o comando (obrigatório escapar com `\`)
    
> "Para cada arquivo encontrado, execute o comando a seguir"


- 5. **`cp -v {} /etc/systemd/system/samba-ad-dc.service`**

Vamos detalhar:

- **`cp`**
	- comando de **copiar arquivo**

- **`-v` (verbose)**
	- mostra o que está acontecendo
-  **-iv**
	- pede confirmação antes de sobrescrever, mostrando o que está acontecendo
 	
> '/caminho/original/samba.service' -> '/etc/systemd/system/samba-ad-dc.service'


- 6. **`{}`**
	- substituído pelo caminho do arquivo encontrado


- 7. **`/etc/systemd/system/samba-ad-dc.service`**
	- destino da cópia
	- também define o **novo nome do arquivo**

> Você está copiando e **renomeando ao mesmo tempo**

##### ⚠️ Ponto MUITO importante

Se o `find` encontrar **mais de um arquivo**, isso vai acontecer:

- Ele vai copiar todos para o **mesmo destino**  
- O último vai sobrescrever os anteriores

#### 💡 Forma mais segura (recomendado)

- Antes de copiar, veja o que será encontrado:
	`find /usr/local/samba-4.23.2/ -type f -iname "samba.service"`

- Depois copie manualmente ou use:
	`find /usr/local/samba-4.23.2/ -type f -iname "samba.service" -exec cp -iv {} /etc/systemd/system/samba-ad-dc.service \;`

### Personalizando e finalizando o arquivo de inicialização

- Ajustando o conteúdo do arquivo copiado.

```bash
# file samba-ad-dc.service

vim samba-ad-dc.service

# Comente ou apague essa linha (11)
# EnvironmentFile=-/usr/local/samba/etc/sysconfig/samba

# altere essa linha (12)
  ExecStart=/usr/local/samba/sbin/samba --foreground --no-process-group -D

```


### Comando re-ler os Daemon
Necessário quando a alteração, inclusão ou exclusão de **unit** no `systemd`

- **Comando para re-ler as configurações do **Daemon**
	- ***`systemctl daemon-reload`***

- **Comando para habilitar o `Daemon`**
	- ***`systemctl enable samba-ad-dc.service`***

- **Verificação do `Daemon` foi realmente habilitado.**
	- **`systectl is-enabled samba-ad-dc.service`**

> se retorna **`enabled`** está tudo OK.

##### ⚠️ Importante ...

Não inicialize a **unit** criada, até que o **DOMÍNIO** esteja instalado ! 

---
---


## **ACTIVE DIRECTORY** 
### Server Roles - Funções do Server Samba (entendimento)

**Server Roles**, corresponde o papel **(função)** que o servidor vai desempenhar na rede.

- **STANDALONE**
	Esse função trabalha como **servidor independente**, possui sua própria base de dados. Para acessar os recursos compartilhado do servidor é necessário primeiro a autenticação, ( _"user + password"_ ) cadastrada na base de dados do servidor para ter acesso aos recursos.

📌 Resumo: 
- Servidor **independente**
- Não participa de domínio
- Usuários locais (Linux)

Quando usar:
- Compartilhamento simples de arquivos
- Laboratórios (tipo o seu cenário com VM)

#### Configuração
Arquivo do samba **`smb_conf`**

`server role = standalone server`

---

- MEMBER SERVER
	Essa função trabalha como **servidor  membro de um domínio**, é necessário que o SAMBA ingresse no domínio (normalmente utilizando o comando `net`). Sua validação ocorre em conjunto com o **CONTROLADOR DO DOMÍNIO**, consultando a base de dados do servidor controlador.

📌 **Resumo**:
- Servidor que **faz parte de um domínio**
- Autenticação feita por outro servidor

📌 Exemplo:
- Integrado com Active Directory

#### Configuração
Arquivo do samba **`smb_conf`**

`server role = member server`

---

- DOMAIN CONTROLLER (DC)
	Essa função trabalha como servidor samba**Controlador de domínio** (Active Directory), para que isso seja possível é necessário promover o servidor a **DC**, usando o comando `samba-tool`

📌 **Resumo**:
- Servidor que **controla o domínio**
- Gerencia:
       - usuários
       - grupos
       - permissões
       - políticas
    
📌 Equivalente ao:
	- Um servidor Windows com AD.

#### Configuração
Arquivo do samba **`smb_conf`**

`server role = active directory domain controller`


### Resumindo :

| Role              | Função                        |
| ----------------- | ----------------------------- |
| Standalone        | Servidor simples, sem domínio |
| Member            | Participa de domínio          |
| Domain Controller | Controla o domínio            |

---
### **ACTIVE DIRECTORY** - Preparando o servidor para instalação

#### **Verificação do KERNEL a suporte a _ACL_ e _Atributos estendidos_.** 

-  **Check - atributos estendidos do `fileSystem` (ativos ou não)**
		`egrep -i "ext4_fs_security" /boot/config-$(uname -r)

> **return:** " CONFIG_EXT4_FS_SECURITY=y"  _y ou n corresponde a compilação OK para atributos estendidos_

- **Check -  ACL do `fileSystem` (ativos ou não)**
	`egrep -i "ext4_fs_posix_acl" /boot/config-$(uname -r)

> **return:** " CONFIG_EXT4_POSIX_ACL=y"  _y ou n corresponde a compilação OK para ACL_


_OBS:. Caso o retorno dos comando fossem diferentes, teria que ser executada a instalação de um novo KERNEL com suporte a **atributos estendidos e ACL**_

---

#### **Verificação,  Montagem da partição (serviço ativo _ACL / ATTR_) **

- **Check partição serviço ativo (ACL / ATTR)**
	`tune2fs -l /dev/sdb1 | egrep -i "mount options"`

> **return:** "Default mount options:    usr_xattr    acl"  _esse retorno corresponde ao serviço ativos na montagem da partição._


_OBS: Feature ativa podemos prosseguir para ativação e instalação do **Active Directory**_

---

### **HOST** - Preparando o servidor para resolução **FQDN**

#### **FQDN** **(Fully Qualified Domain Name)** - Nome de Domínio Totalmente Qualificado

 **FQDN**, é o nome completo de um host na rede, incluindo:

- nome da máquina (hostname)
- domínio
- domínio de topo (TLD)

	**Exemplo:** `srvdc1.empresa.local`

**Aqui:**
- `srvdc1` → hostname (nome da máquina)
- `empresa` → domínio
- `local` → domínio de topo (TLD)

O **FQDN** é usado para: 
- Identificar máquinas de forma **única na rede/internet**
- Configurações de serviços como:
    - **Samba AD DC**
    - servidores web (Apache, Nginx)
    - DNS
- Certificados SSL (HTTPS)

#### **Comando para visualizar o **FQDN**
	`hostname -f`

#### Arquivo  **HOSTs** ( `/etc/hosts` )
Alterando arquivo **`hosts`**

```bash
# Alterar linha 2
vim /etc/hosts
	
1
2 10.10.10.11    srvdc1.empresa.local    srvdc1
3
...	
	
:set nu # numera linha para visualização.
```

---

### **DNS** (port 53)

Um **ACTIVE DIRECTORY** depende exclusivamente de um serviço de **DNS**, e quando você instala um **SERVIDOR SAMBA** ele ja instala um servico de **DNS**, porém o próprio **O.S** (sistema operacional) ja tem um serviço de **DNS** rodando inclusivel na mesma porta, o que ocasiona conflito com o **SERVIDOR SAMBA**.

> É necessário desativação do serviço client **DNS** do sistemas operacional e ativação do **DNS** do **SERVIDOR SAMBA**


#### Verificando PORTAS (comando `ss`)

O comando **`ss`** (socket statistics), usada para **visualizar conexões de rede e sockets**. Ele é o substituto moderno do antigo `netstat` (mais rápido e completo).

#### O que o `ss` mostra?
Com ele você consegue ver:

- conexões TCP e UDP
- portas abertas
- serviços escutando (listening)
- IPs conectados
- processos que estão usando a rede

#### Utilizando o comando

- **Ver todas as conexões**
	`ss`

- **Ver apenas conexões TCP**
	`ss -t`

- **Ver apenas conexões UDP**
	`ss -u`

- **Ver portas em escuta (muito útil pra servidor)**
	`ss -l`

- **Ver tudo com mais detalhes** 
	`ss -tuln`

- **Visualizar processo que está usando a porta**
	`ss -tulnp`

	**return:**
	  `LISTEN  0  128  0.0.0.0:22  0.0.0.0:*  users:(("sshd",pid=1234))`
	
👉 Aqui mostra que:
	- porta **22** está aberta
    - serviço: **sshd**


**Esse é o mais usado no dia a dia**

- `-t` → TCP
- `-u` → UDP
- `-l` → listening
- `-n` → não resolve nomes (mais rápido)

---

#### Verificando  port 53 ( Usada pelo Active Directory) - serviço DNS
	`ss tuln | grep 53` 


### Desabilitando o serviço na porta 53
(deixando port livre para o **Active Directory**)

	`sytemctl stop systemd-resolved.service`
	`systemctl mask systemd-resolved.service`


---

### **Habilitando o sistema a utilizar DNS** - "Active Directory"

#### Editando arquivo de **network** (rede) **`/etc/netplan/50-cloud-init.yaml`**
"Name Servers"

```bash
# Alterar linha 6 (end de DNS - direcionar para o server)
vim /etc/netplan/50-cloud-init.yaml

network:
	version: 2
	ethernets:
		enp0s3:
			addresses:
			- "10.10.10.11/24"
			nameservers:
				addresses:
				- 127.0.0.1
				search:
				- empresa.local
			routes:
			- to: "default"
			  via: "10.10.10.1"


:set nu # numeração linha (visualização)

```


#### Habilitando o arquivo alterado.
	`netplan apply`


---

#### ARQUIVO DE CONFIGURAÇÃO **DNS** do sistema (`/etc/resolve.conf`)

Alterando para habilitar o DNS do próprio servidor.
Redirecionamento ao **`nameservers`**

```bash

# segurança backup
mv -v /etc/resolv.conf /etc/resolv.conf.bkp

# alterando conteudo do /etc/resolv.conf
echo "nameserver 127.0.0.1" > /etc/resolv.conf

```

---

#### **INSTALL ACTIVE DIRECT** (Domain Controller)
domain
- **Install**
	`samba-tool domain provision --use-rfc2307 --interactive`

- Sobre escrevendo o arquivo **krb5.conf**
	`cp -vb /usr/local/samba/var/lib/samba/private/krb5.conf /etc/`

> **cp -vb**  "cria uma copia do arquivo indicado para o local especificado. Constando um arquivo no local especificado com o mesmo nome, ele substituirá o arquivo existente, criando uma copia do arquivo com o simbolo de "~" ao final do nome do arquivo."

##### **Entendimento:**

O arquivo **`krb5.conf`** é o principal arquivo de configuração do **Kerberos** em sistemas Linux.

Ele define **como o sistema vai se autenticar usando Kerberos**, muito comum em ambientes com:

- **Samba AD (Active Directory Domain Controller)**
- Integração com domínio Windows
- Serviços centralizados de autenticação


**Kerberos** é um protocolo de autenticação que usa **tickets criptografados** para evitar envio de senha em texto claro.

---

#### SMB.CONF (arquivo principal do Servidor Samba)

- Qualquer tipo de alteração no arquivo `smb.conf`,  o mesmo tem que ser **restartado**
	`systemctl restart samba-ad-dc.service`


- Configurações **\[Globais\]** úteis

	`dns forwarder = 8.8.8.8`  

> Corresponde ao endereço IP de quem irá traduzir/ resolver, resolução de nomes externos (DNS)

DNS (_Domain Name System_) é um sistema que **traduz nomes de domínio em endereços IP**.

---

### SERVICE **NTP** (sincronização horas )

**NTP** significa _Network Time Protocol_ ou **Protocolo de Tempo para Redes**. É o padrão que permite a sincronização dos relógios dos dispositivos de uma rede como servidores, estações de trabalho, roteadores e outros equipamentos à partir de referências de tempo confiáveis. Além do protocolo de comunicação em si, o NTP define uma série de algoritmos utilizados para consultar os servidores, calcular a diferença de tempo e estimar um erro, escolher as melhores referências e ajustar o relógio local.

#### Instalação do serviço (chrony)

	`apt-get install chrony`


#### Ajustando arquivo de configuração do serviço (chrony)
`/etc/chrony/chrony.conf


```bash
vim /etc/chrony/chrony.conf

# Comment os server Pool (all)
# ative esse no lugar

server a.st1.ntp.br iburst nts
server b.st1.ntp.br iburst nts
server c.st1.ntp.br iburst nts
server d.st1.ntp.br iburst nts
server e.st1.ntp.br iburst nts
server gps.nu.ntp.br iburst nts
server gps.jd.ntp.br iburst nts
server gps.ce.ntp.br iburst nts

# Habilitar Serviço NTP Chrony - Network empres.local
allow 127.0.0.1
allow 10.10.10.0/24

```


#### Habilitar/verificação `chrony.service`

- Restart serviço
	`systemctl restart chrony.service`

- Verificação da ativação.
	`systemctl status chrony.service`

- Habilitar (caso esteja desabilitado)
	`systemctl enable chrony.service`


- Verificar inicialização (boot)
	`systemctl is-enabled chrony.service`


---

#### SHARED FILES (DC_AD) - Compartilhamentos no Samba para DC_AD

**Entendimento!**
	que se possa gerenciar compartilhamentos as ACL de segurança 

- **Privilégio  de gerenciamento as permissões de compartilhamento** (adicionar)
	`net rpc rights grant administrators SeDiskOperatorPrivilege -U adminstrator`

- **Privilege list concedidos** (lista privileges concedidos)
	`net rpc rights list privileges SetDiskOperatorPrivilege -U administrator`

- Creator directory
	`mkdir -vm 770 /mnt/samba`

- Visualizando informações de user ou group cadastrado no DC_AD
	`wbinfo --group-info 'builtin\administrators`

- Alterando proprietário group folder
	`chgrp -v GID /mnt/samba 
_`GID do grupo a qual deseja conseder propriedade`_


- **Criando o SHARED no samba (compartilhamento basic)**
	Arquivo `smb.conf`

```bash

[TI]
	path = /mnt/samba
	read only = no

```

> path => caminho do local onde compartilhado
> read only => indicação que o compartilhamento NÃO SERÁ somente leitura e sim leitura e escrita.


- **Testando configurações SAMBA**
	`testparm`
	`smbcontrol all reload-config` 

- Lista MEMBER Group ***DC_AD*** (user que tem permissões as configurações de compartilhamentos)
	`samba-tool group listmembers administrators`

-**ADD account user Samba.**
	`samba-tool user create userTest1`


---
---


### Ingressando Maquina linux no DOMÍNIO - DC-AD

- **Install package**
	`apt install realmd sssd sssd-tools adcli`


- Alteração na **Network**(rede) - (`NetWorkManeger`)
	`nmtui`
	`systemctl restart NetWorkManager.service`


### NTP

- **NTP (time date)**

	**Client NTP default** 
	arquivo: `/etc/systemd/timesyncd.conf` 
	serviço: `timesyncd.service`

	**Configurando o NTP para sincronismo com o Controller DC-AD**

	- Crie folder
		`mkdir -v /etc/systemd/sytemd.timesyncd.conf.d`

	- Crie file (`samab.conf`).
		`touch /etc/systemd/sytemd.timesyncd.conf.d/samba.conf`
	
	- Conteúdo
	
	```bash
	vim samba.conf
	
	[Time]
	NTP=10.10.10.11
	
	# NTP=10.10.10.11 10.10.10.12 IPs servidores DC - NTP
	
	```

**OBS:** _Esse processo de criar uma pasta com o mesmo nome do arquivo **default NTP**, e dentro um outro arquivo contendo as configurações necessárias, **serve para evitar que as configurações inseridas ao NTP(direcionamento), seja sobrescritas.**_

- Restarta serviço **NTP** 
		`systemct restart systemd-timesyncd.service`
		`systemct stauts systemd-timesyncd.service`


- Testando connection (**server communication**)
	``realm -v discover empresa.local`


- Ingressar maquino no **DOMÍNIO**
	`realm join empresa.local --user=administrator --client-software=sssd --os-name="Ubuntu Linux" --os-version="24.04" -v `

- Personalizando **/home DC-AD** dos usuários
	Arquivo default `/etc/sssh/sssh_conf`

```bash
vim /etc/sssh/sssh_conf

# adicionar essa linha de comando (ultima)
override homedir = /home/%d%u 

```

- Serviço **SSSH**  "restart/ habilita" para inicialização no **boot**
	``systemctl restart sssh.service`
	`systemctl restart sssh.service`

- Configuração modo de **Local Authentic** (modos do PAM - autenticação modo local)
	\# _active home directory on login_
	`pam-auth-update` 
	`restart`

---
---


