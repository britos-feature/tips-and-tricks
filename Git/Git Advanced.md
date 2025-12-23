# **Repositório GitHub diretamente pelo terminal**

^be26d9

Para manipular **GitHub (web)** pelo **terminal**  é necessário utilizar o **GitHub CLI (Command Line Interface)** — uma ferramenta oficial da própria GitHub.

- #### Instalar o _GitHub CLI_

```sh

sudo apt install gh -y

```
<br> **Verificação de versão**

```sh

gh --version

```
<br>
- #### Fazer login no GitHub

```sh

gh auth login

```
<br>**Durante o processo:**

- Escolha: **GitHub.com**
- Método de autenticação: **HTTPS**
- E selecione **Login with a web browser** (ele abrirá uma janela para você autenticar)
- Depois disso, o CLI fica autorizado a criar e gerenciar repositórios.


- #### Criar o repositório direto do terminal (processo de criação)

```sh

gh repo create project # Inside the project folder (dentro da pasta do project)

```
<br>**Perguntas:

```css
/* Selecione opção desejada: */

> Public
  Private
  Internal

This will add a remote to your local git repository. Continue? (Y/n)

```
<br>
- #### Enviar seus arquivos (caso ainda não tenha feito)

```sh 

git add .
git commit -m "First commit"
git push -u origin main

```
<br>
<h4><span style="color:red"><big>Dica extra:</big></span> tudo em um comando só</h4>
	- Se quiser fazer tudo automático (**sem perguntas**):

```sh

gh repo create meu-repositorio --public --source=. --remote=origin --push

```


---

### **Visualizando log de alteração feita antes de comitar**

- **GIT DIFF**
	O comando `git diff` é usado para visualizar as diferenças entre as versões dos arquivos no repositório. Ele mostra quais linhas foram adicionadas, removidas ou modificadas antes de você fazer um commit.

```bash
git diff
# Ver diferenças em arquivos modificados (mas ainda não adicionados ao stage)

git diff --staged
# Comparar mudanças que já foram adicionadas ao stage

git diff rashCommit1 rashCommit2
# Compara dois commits específicos

git diff origin/main
# Compara com a versão remota

git diff branch1 branch2
# Comparar mudanças entre branches

git diff --name-only
# Visualiza nome dos arquivo que houveram alterações
```


- **GIT CHECKOUT**
	Deletando/remove ultimo estado do arquivo (até ficar em seu estado inicial)

```bash
git checkout -- file 
# Descartar as modificações locais do arquivo específico e restaurá-lo para o último estado salvo no repositório.

	# Se o arquivo foi modificado, mas não commitado, ele volta à versão mais recente do último commit.

	# Se o arquivo for novo (não rastreado pelo Git), o comando não funciona (você deve deletá-lo manualmente).
```

> **<span style="color: red;">CUIDADO! </span>** Este comando **não pode ser desfeito!** Se você rodar `git checkout -- arquivo`, as alterações serão **perdidas para sempre** (a menos que estejam em outro commit ou no stash).


- **GIT STASH**
	O **`git stash`** é um recurso do Git que permite **salvar temporariamente** as alterações sem fazer um commit. Isso é útil quando você precisa mudar de **branch** ou restaurar o estado anterior sem perder o trabalho feito..

```bash
git stash
# Isso guarda todas as modificações nos arquivos rastreados e restaura o diretório para o último commit.

# Arquivos novos e não rastreados não são salvos no stash!
```


- **Listar os stashes armazenados**

```bash
git stash list
# Exibe todos os stashes salvos, por exemplo:
	# stash@{0}: WIP on main: 34ac1c2 Corrigindo bug no login
	# stash@{1}: WIP on main: f5b2a8d Ajustando estilos CSS
```


- **Restaurar o stash mais recente**

```bash
git stash pop
# Aplica as alterações do stash mais recente (`stash@{0}`) e **remove** ele da lista.

git stash apply
# Aplica as alterações do stash mais recente (`stash@{0}`) mantendo-o na lista.
```


- **Restaurar um stash específico**

```bash
git stash apply stash@{1}
# Se houver vários stashes e quiser restaurar um específico, veja a lista com `git stash list` e use. 
```


- **Remover um stash específico**

```bash
git stash drop stash@{0}
# Isso exclui um stash sem aplicá-lo.
```


 - **Limpar todos os stashes armazenados**

```bash
git stash clear
# Isso remove todos os stashes de forma permanente!
```


#### **Exemplo prático**

```bash
# Você modificou arquivos, mas ainda não quer commitá-los
git stash

# Você muda de branch para corrigir algo
git checkout outra-branch

# Depois de terminar, volta para a branch original e recupera o stash
git checkout main
git stash pop

```


### **Git reset**

O comando **`git reset`** é uma ferramenta poderosa do Git usada para desfazer mudanças no repositório. Ele pode ser usado para mover a **HEAD** e, dependendo das opções, também alterar o estado do índice (staging area) e do diretório de trabalho (working directory)

#### **Sintaxe básica**

```bash
git reset [opções] [commit]
```

#### **Opções**

- #### **--soft**

```bash
git reset --soft [commit]
# - Move a HEAD para `<commit>`, mas mantém as mudanças no staging e no working directory. Ideal para desfazer commits sem perder mudanças.

# Exemplo:
git reset --soft HEAD~1  # Remove o último commit, mas mantém as alterações no staging
```


- #### **--mixed (default)**

```bash
git reset --mixed [commit]
# - Move a HEAD e desfaz as mudanças do staging (index), mas mantém no working directory. Ideal quando você quer refazer um commit, mas manter as mudanças no código.

# Exemplo:
git reset --mixed HEAD~1  # Remove o último commit e retorna as mudanças para working directory
```


- #### **--hard**

```bash
git reset --hard [commit]
# - Move a HEAD, remove as mudanças do staging e também apaga as modificações do working directory. Ideal para descartar completamente alterações.

# Exemplo:
git reset --hard HEAD~1  # Apaga o último commit e todas as mudanças no código
```


