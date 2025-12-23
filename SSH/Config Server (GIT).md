
## ***Configuração (client)*** 
*client (local)*

Passo a passo

1. Instalação GIT (necessário ***Server/ Client***)

```shel
sudo apt install git
```

2. Configuração GIT ***client*** (name, email)

```shell
git config --global user.emal "email@email.com"
git config --global user.name "Name Lastname"
```

3. Inicialização do GIT ***client***

```shell
git intit
```

4. Create file **`.gitignore`** on ***client***

```shell
touch .gitignore
# add files, folders to .gitignore file
```

5. Adicionando repository GIT remoto do ***server***  no ***client*** 

```shell
git remote add nameRepository user@IP:path_absolute
# example:
# git remote add boilerplate britos@192.168.1.8:/home/britos/Documents/repositories/repo-boilerplate
```

## ***Configuração (server)***
*server (remoto)*

Passo a passo

1. Instalação GIT (necessário ***Server/ Client***)

```shel
sudo apt install git
```

2. Create repositories GIT on ***server***

```shell
mkdir nameProject repo-nameProject
```

3. Inicialização do GIT ***server***

```shell
git intit --bare
# command used in folder repo-nameProject

git init
# command used in folder nameProject
```

4. Adicionando repository remoto GIT ***server***

```shell
git remote add path_absolute_folder
# git remote add nameProject /home/user/repo-nameProject
```


## ***Utilização***

***client versionando arquivo***

```shell
git add .
git status
git commit -am "Commit initlal"
git log # vizualização commits
git log --oneline # vizualizaçã por linhas commits

git push nameProject master # enviando
# nameProject = repository que ira receber o branch descriminado
# master = name do branch
```


***server versionamento arquivo***

```shell
git pull nameProject master # recebendo
# exec command in folder nameProject
```


***Úteis***

 - Vizualizar repositories remotos

```shell
git remote -v
```

- Mostrar mais informações sobre repositories remoto

```shell
git remote show nameRepository
```

- Remover repositories remotos

```shell
git remote remove nameRepository
# or
git remote rm nameRepository
```


