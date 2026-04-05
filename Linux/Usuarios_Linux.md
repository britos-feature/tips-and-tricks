
# ***All about user (Tudo sobre user)***
Dicas para usuários no **linux**

## Comando para verificação de usuários

### **`id`**  
Lista o seu **UID**, **GID** e os grupos que o user pertence.

- **`id joao`**

> return esperado:  **UID** (id usuário), **GID** (id do grupo) e grupos a qual pertence.


### `chwon`
Troca o **usuário** ou **grupo** "DONO" do arquivo ou diretório

 - `chown joao file.txt`   ->   _trocou  o dono do arquivo para o user "joao"_
 - `chown joao:adm file.txt`   ->    _trocou  o dono/grupo do arquivo para o user "joao"  group "adm"_


### `userdel`
Deleta o usuário, permanecendo o **`/home`**

- `userdel joao`


### `deluser`
Deleta o usuário do **GRUPO** referenciado.

- `deluser joao sudo`   ->   user "**joao**", não pertence mais ao grupo "**sudo**"

### `usermod` 
Modifica dados do usuário como: _"**`name`**", "**`/home`**", "**`shell`**"._

- `usermod -l maria joao`   ->    altera o nome **joao** para nome **"maria"**.
- `usermod -d /home/other -m joao`   ->   altera `/home` do user "**joao**"  para "**/home/other**"
- `usermode -s /usr/bin/bash joao`   ->   altera o `/shell` do user "**joao**" para "**/usr/bin/bash**"
- `usermod -aG sudo joao`    ->    adiciona o grupo "**sudo**" ao user "**joao**" <br> _**a** =  ("append)   **G** = ("group")_


### `getent`
Comando que serve para **consultar bancos de dados do sistema** (usuários, grupos, hosts, etc.) de forma padronizada.

- **`getent`** ->   exemplo: `getent passwd joao
	return esperado: linha do conteúdo do **banco de dados** `/etc/passwd`


### Bancos mais usados

**BANCO DE DADOS            DESCRIÇÃO**
passwd                ->           usuários /etc/passwd
group                   ->           grupos /etc/groups
shadow                ->          senhas /etc/shadow (root apenas)
hosts                    ->          dns do serviços externos
services               ->          portas e serviços

---


## Criando Usuários (`useradd`) 

**Exemplo:**

	`britos:x:1000:1000:Alexandre de Brito:/home/britos:/usr/bin/zsh`

**Fatiando campos:**

- **`britos`** -> username do usuário
- **`x`** -> senha (`x` , significa que existe uma senha cifrada(configurada) em `/etc/shadow`)
- **`1000`** -> UID (id do usuário)
- **`1000`** -> GUI (id do grupo do usuário)
- **`Alexandre de Brito`** -> Comentários (opcional)
- **`/home/britos`** -> diretório **home** do usuário
- **`/usr/bin/zsh`** -> path(caminho) para um comando , ou shell do usuário

#### Comando para se criar usuário no Linux

#### `useradd` _(comando mais básico, porém poderoso)_


- **Basic**
	
	`useradd Joao`  
	
> 👉 Isso cria o usuário **sem senha e sem diretório home** (dependendo da distro). Necessário  privilégio administrativo (root)


- **Completo**

	`useradd -m -s /usr/bin/zsh joao` **ou**
	`useradd -ms /usr/bin/zsh joao`

> 👉 Isso cria um usuário completo (`/home` e `shell`).
>  Basta agora só criar uma senha ( `passwd joao` )
> Necessário  privilégio administrativo (root)


- **Especificando `UID` e `GID`**

	`useradd -u 1500 -g 1500 -ms /usr/bin/zsh joao`

> 👉 Cria usuário especificando o seu `UID` / `GID` e completando com o (`/home` e `shell`).
>  Necessário  privilégio administrativo (root)


- **Já adicionando em grupos**

	`useradd -ms /usr/bin/zsh joao -G sudo,users`

> 👉 Cria um usuário completo (`/home` e `shell`) e adiciona-lo nos `groups`(`sudo, users`)
> Necessário  privilégio administrativo (root)


- **Sem login (serviço)

	`useradd -r -s /usr/bin/nologin user_service`

> 👉 Cria o usuário **sem login**, muito usado para serviços (ex: nginx, samba, etc.).
> **`-r`** -> user system
> **`-s`** -> impede o login 
> Necessário  privilégio administrativo (root)


- **Usuário com `/home` customizado**

	`useradd -m -d /data/joao joao`

---

#### `adduser` _(mais amigável  -> "interactive")_

	`adduser joao`

> Ele vai pedir:  senha, nome completo, outras infos (opcional)
👉 Esse é o mais recomendado para iniciantes.

---

## Adicionando grupos ao usuário (`usermod`)

- ***Add user in group*** *(adicionando usuário a grupos)*

```shell
sudo usermod -aG nameGroup nameUser
# usuário terá permissões administrador
```

- ***Del user*** *(Remover usuários)*

```shell
sudo deluser nameUser

sudo deluser --remove-home nameUser
```

- ***Group list*** *(Listagem dos grupos)*

```shell
groups
```

- ***All list system user*** *(Listagem de todos user do sistema)*
	*Arquivo /etc/passwd*

```shell
cut -d: -f1 /etc/passwd
```


---

## Usuários GIT (dicas)
Usuário chamado `git` (padrão para repositórios Git)

```shell
sudo adduser git --disabled-password --gecos ""
```

> ***`--disabled-password`*** → o usuário **não terá senha**, só poderá logar via **chave SSH**. <br>
> ***`--gecos ""`*** → pula as perguntas de nome, telefone, etc.

'Depois você deve copiar a sua **chave SSH pública** para o usuário `git`, para poder acessar via `ssh git@servidor`.'

Esse usuário geralmente é usado para serviços como **Gitolite**, **Gitea**, **GitLab**, ou repositórios bare:

- Não precisa de shell interativo (pode usar `/usr/bin/git-shell` para segurança extra)
- Não precisa de senha.
- Você coloca suas chaves SSH no arquivo.

```shell
/home/git/.ssh/authorized_keys
```



