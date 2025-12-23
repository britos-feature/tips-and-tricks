# GIT

É uma ferramenta de controle de versionamento de arquivos. GIT, em algumas OS por padrão já vem instalado como no LINUX

> **OBS:** <span style="color: red;">Git é diferente de GitHub.</span>

### **Entendimento GIT (working directory/ Stage / Committed)**

- Git trabalha dessa forma, passando por um ciclo de **três áreas** antes de ser registrado:

  1.  **Working Directory (Diretório de Trabalho)** → Onde os arquivos são editados.
  2.  **Stage (Área de Preparação)** → Onde os arquivos são adicionados antes do commit.
  3.  **Local Repository (Repositório Local)** → Onde os commits são armazenados permanentemente (repositório).

      GIT --> STAGE --> .GIT

- Para registrar uma mudança, o processo é:

  1.  **Modificar Arquivos** no diretório de trabalho.
  2.  **Adicionar ao Staging** com `git add`.
  3.  **Criar um commit** com `git commit`.

#### **Significados:**

- **Working dir / directory** = diretório de trabalho (project)
- **Stage** = área que **GIT** reconhece aquivos do **Working directory** (necessário add arquivos)
- **Committed** = arquivos prontos, atualizados e dentro do repositório **versionado** marcado como **HEAD** identificação de ultima versão.

### Arquivos de configuração Git

Git utiliza vários arquivos de configuração para gerenciar suas configurações e preferências.
Os principais são:

**`.git/config:`** este é o arquivo de configuração local de um repositório. Ele armazena configurações específicas para aquele repositório, como remotos, branches e opções específicas do projeto.

**`~/.gitconfig`** ou **`~/.config/git/config`:** este é o arquivo de configuração global do usuário. As configurações aqui se aplicam a todos os repositórios do usuário no sistema. Pode-se incluir informações como nome de usuário e e-mail, preferências de merge, entre outras.

**`/etc/gitconfig`:** este é o arquivo de configuração do sistema. As configurações aqui se aplicam a todos os usuários e repositórios no sistema. É usado para definir configurações que são comuns a todos os usuários.

Git também permite que você especifique configurações temporárias diretamente na linha de comando usando as opções:
**`git config --global`**
**`git config --system`**
**`git config --local`**

### Status dos arquivos no repositório(local) Git

Explicação e entendimento do Ciclo de vida GIT (status dos arquivos no repositório)

**untracked** = arquivos do diretório de trabalho, (NO STAGE - Git não reconhece)
**unmodified** = arquivo no STAGE GIT pronto para serem comitted
**modified** = indica alguma modificação no arquivo que ja se encontra no STAGE - porém precisa ser realox
**staged** = arquivo reconhecido no GIT repositório e versionado/ **commitado**

### Configuração inicial Git

- Definições de user

```bash
git config --global user.name "nameUser"
```

- Definições de email

```bash
git config --global user.email "email@email.com"
```

- Definição para editor

```bash
git config --global core.editor subl # sublime
```

- Conferindo configurações

```bash
git config user.name
git config user.email
git config core.editor

git config --list # configurações geral (local)
	git config --global --list # configurações
```

### Criação e inicialização de repositório(local) Git

A criação de um repositório local é a sua inicialização em uma pasta/folder local, onde serão efetuados os versionamentos dos arquivos.

```bash
mkdir myProject
cd myProjec
git init # inicialização do Git
```

### Clonando repositório (local) Git

A clonagem de um repositório localmente é a copia da pasta de arquivos, inicializando o Git para poderem ser efetuados os controles de versionamentos da pasta de arquivo copiada.

```bash
git clone /caminho/pasta # clone copia

git clone /caminho/pasta /caminho/newPasta # clone para pasta diferente
```

### Comandos básicos para repositórios (local) Git

- **Verificação do status do GIT (files)**

```bash
git status
```

- **Adicionar arquivo no STAGE do repo GIT**

```bash
git add "file"
```

- **Removendo arquivo do STAGE do repo GIT**

```bash
git reset file
```

- **Restaurando arquivo status <span style="color:red";>MODIFIED</span> (NO STAGE)**

```bash
git restore file
```

> _`git restore`_ é utilizado para quando o arquivo ja se encontra no STAGE mais ainda "não foi commitado", mais houve alteração sinalizando como um arquivo _`MODIFIED`_, sendo necessário sua inclusão no STAGE para atualização do mesmo. Utilizamos _`git restore`_ para reverte as alterações permanecendo o arquivo original sem alterações.

#### Commit

No **Git**, um **commit** é um registro de mudanças no repositório **(versionamento)**. Ele funciona como um "instantâneo" do código em um determinado momento. Cada commit contém informações sobre quais arquivos foram alterados, quem fez a alteração e uma mensagem descritiva da mudança.

- **Versionando arquivo do STAGED no repo GIT**

```bash
git commit -m "comentario" (para arquivos novos)
git commit -am "comentario" (para arquivos ja existente)

# -a ou --all = Adiciona automaticamente ao commit todas as alterações feitas em arquivos que já estavam sendo rastreados pelo Git.
# -m ou --message = Permite adicionar uma mensagem ao commit diretamente no comando.

# A diferença é que (git commit -am) não adiciona arquivos novos que ainda não foram rastreados pelo Git. Para novos arquivos, você ainda precisa usar (git add file) antes.
```

- **Alterar a Mensagem do Último Commit**

```bash
git commit --amend -m "Nova mensagem corrigida"
# Se cometeu um erro na mensagem do commit, pode corrigir assim.
```

- **Reverter um commit sem perder as alterações**

```bash
git reset --soft HEAD~1
```

- **Excluir um commit e as alterações**

```bash
git reset --hard HEAD~1
```

**Remove arquivo do Staged**
_`git reset HEAD file`_

**Remove estado file (cuidado)**
_`git reset [--soft, --mix, --hard] numero_rash`_

%%

- --soft remove commit, deixando o arquivo pronto para ser comitado novamente com a (modified) anterior a ultima modified
- --mix remove o commit, deixando o arquivo no estado de (modified)
- --hard remove o commit, deixando o arquivo em seu estado inicial.
  %%

<span style="color:red">CUIDADO!</span> _`git reset hard`_ altera o histórico do **GIT**, sendo necessário atualizar o histórico do GIT com 'FORCE'

- **Visualizando log**

```bash
git log # default

git log --oneline # visualização minificada em 1 linha

git shortlog # versão minimizada de todos comites de todos os authores

git shortlog -sn # resumo de log dos authores e qtd de commit realizados

git log --decorate # log com especificação dos branch

git log --author="nameAuthor" # log só do author especificado

git log --graph # modo graficos dos log efetuados pelos authores

git log show "number_Rash" # modo detalhado dos commit referente ao rash informado
```

### [[Git Advanced | Comandos Avançados Git]]

# GITHUB

É uma plataforma (web) de hospedagem de arquivos, código-fonte e colaboração, baseada no sistema de controle de versões Git, onde podemos criar repositórios.

### **Principais Conceitos do GitHub**

#### **Repositórios (Repositories)**

Um repositório é onde o código do seu projeto fica armazenado. Pode ser **público** (qualquer um pode ver) ou **privado** (apenas pessoas autorizadas têm acesso).

#### **Commits e Histórico**

Cada mudança no código pode ser registrada com um **commit**, que contém uma mensagem explicando o que foi alterado. Assim, você pode acompanhar a evolução do projeto.

#### **Branches (Ramificações)**

Uma **branch** permite criar versões paralelas do projeto sem afetar a versão principal (geralmente a `main` ou `master`). Isso é útil para desenvolver novas funcionalidades sem comprometer o código original.

#### **Pull Requests (PRs)**

Quando alguém quer unir uma branch ao código principal, faz um **pull request (PR)**. Isso permite que outros revisem as mudanças antes da fusão (merge).

#### **Forks e Contribuições**

Você pode fazer um **fork** de um repositório para criar uma cópia dele em sua conta, modificar o código e, se desejar, sugerir mudanças ao projeto original através de um **pull request**.

#### **Issues**

O GitHub tem um sistema de **issues**, que funciona como um quadro de tarefas para relatar bugs, sugerir melhorias ou organizar o trabalho do time.

#### **GitHub Actions**

Permite automatizar tarefas, como testes e deploys, sem precisar fazer manualmente cada vez que houver uma alteração no código.

### **GitHub na Prática**

- #### [[Git Advanced#^be26d9|Criar um repositório no GitHub pelo terminal]]

- #### **Criar um Repositório no GitHub** (web)

  1.  Acesse [GitHub](https://github.com/) e faça login.
  2.  Clique no botão **New** (ou "Novo Repositório").
  3.  Escolha um nome para o repositório (ex: `meu-projeto`).
  4.  Defina se ele será **público** ou **privado**.
  5.  Marque a opção **Add a README.md file** (opcional).
  6.  Clique em **Create repository**.

- **Clonar o Repositório**
  Depois de criar o repositório no GitHub, copie a **URL do repositório**

```bash
git clone https://github.com/seu-usuario/meu-projeto.git
cd meu-projeto

git clone https://github.com/seu-usuario/meu-projeto.git myLocal
cd myLocal
```

- **Adicionar um arquivo e enviar para o GitHub**

```bash
git add .
git commit -m "Adicionando um novo arquivo"
git push origin main
```

- **Enviar para o GitHub (Push)**
  Se você clonou o repositório, basta rodar

```bash
git push origin main
```

- **Se criou o repositório localmente, precisa conectar ao GitHub antes**
  - Detalhes sobre **_git remote_** -> **[[Git Remote| Guia repositório remoto]]**

```bash
git remote add origin https://github.com/seu-usuario/meu-projeto.git
git branch -M main
git push -u origin main

```

- **Criar e mudar para uma nova branch**

```bash
git checkout -b nova-branch
```

### **Fork**

O **fork** é uma cópia de um repositório GitHub para sua conta. Ele permite que você:

- Teste alterações sem afetar o projeto original.
- Faça melhorias ou correções e sugira que o dono do projeto as adicione.

#### **Como fazer um Fork**

1. Vá até um repositório público no GitHub.
2. Clique no botão **Fork** no canto superior direito.
3. Isso criará uma cópia do repositório em sua conta.

Agora você pode clonar o repositório para seu computador e fazer alterações!

```bash
git clone https://github.com/seu-usuario/projeto-forkado.git
cd projeto-forkado
```

### \*_Pull Request (PR)?_

Um **Pull Request** é uma solicitação para mesclar (merge) suas alterações ao projeto original. Ele permite que os donos do repositório revisem e aprovem as mudanças antes de incorporá-las.

#### **Fazer um Pull Request**

Crie uma nova branch para suas alterações

```bash
git checkout -b minha-nova-feature
# Isso mantém suas mudanças separadas do código principal.
```

#### **Faça alterações e um commit:**

Edite o código e depois use

```bash
git add .
git commit -m "Adicionei uma nova funcionalidade"
```

#### **Envie para o GitHub (Push):**

```bash
git push origin minha-nova-feature
```

#### **Crie o Pull Request no GitHub:**

- Vá até o repositório no GitHub.
- Clique em **Compare & pull request**.
- Adicione uma descrição e clique em **Create pull request**.

Agora, o dono do projeto pode revisar e decidir se aceita suas alterações!

### **Resumo**

| Conceito         | O que faz?                                                        |
| ---------------- | ----------------------------------------------------------------- |
| **Fork**         | Cria uma cópia do repositório na sua conta.                       |
| **Branch**       | Cria uma nova ramificação para alterações.                        |
| **Pull Request** | Solicita que suas alterações sejam mescladas ao projeto original. |

---

---

[^1]: Versionamento, é o processo de gerenciar diferentes versões de um arquivo, código ou sistema ao longo do tempo. Ele permite acompanhar as mudanças feitas, reverter para versões anteriores se necessário e colaborar com outras pessoas de forma organizada.
[^2]: Git é um repositório local em seu computador, enquanto GitHub é um repositório remoto(web).
