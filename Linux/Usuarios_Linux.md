# ***All about user (Tudo sobre user)***
Dicas para usuários no **linux**

- ***Create user*** *(Criação de usuário comum)*

```shell
sudo adduser nameUser
sudo adduser --no-create-home nome_do_usuario # sem /home
# or
sudo useradd nameUser # no question (without /home directory)
```

> ***`adduser`*** = sistema vai te pedir uma senha e alguns dados (nome, telefone, etc — você pode só apertar Enter). <br>
> ***`useradd`*** = sistema cria automaticamente usuário sem diretório ***`/home`*** 


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



