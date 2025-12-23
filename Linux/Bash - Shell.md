O **Bash** é um **interprete de comandos** (ou _shell_) muito popular no mundo Linux e Unix. Basicamente, ele é o programa que lê os comandos que você digita no terminal e os executa.

Detalhando o **bash**:

1. **Nome**:
       - Bash significa **Bourne Again Shell**.
       - É uma versão melhorada do shell original chamado **Bourne Shell (sh)**.
        
2. **Função principal**:
       - Interpretar comandos que você digita no terminal.
       - Executar scripts (_programas em texto_) escritos em linguagem de shell.
        
3. **O que você pode fazer com Bash**:
       - Navegar em diretórios (`cd`, `ls`).
       - Manipular arquivos (`cp`, `mv`, `rm`).
       - Executar programas.
       - Automatizar tarefas criando scripts (`.sh`).
       - Criar variáveis, loops, condicionais, funções, etc.
        
4. **Por que é importante**:
       - É a base para administração de sistemas Linux/Unix.
       - Permite automação de tarefas repetitivas.
       - É muito usado por desenvolvedores, administradores e DevOps.

---

O Bash possui **muitas opções** que alteram o comportamento do shell ao ser iniciado. Vou detalhar as principais e mais usadas, organizadas por categorias:

#### - ***Opções de execução de comandos***

| Opção             | Descrição                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------ |
| `-c string`       | Executa os comandos contidos em `string`.                                                  |
| `-i`              | Força o shell a ser **interativo**.                                                        |
| `-l` ou `--login` | Inicia como um **shell de login**, lendo arquivos como `/etc/profile` e `~/.bash_profile`. |
| `-s`              | Lê comandos da **entrada padrão (stdin)**.                                                 |
<br>
**exemplos:**

```sh
bash -c "echo Olá Mundo"
echo "echo Olá" | bash -s

bash -il
```
<br>
#### Mais detalhes sobre o **[[Command echo - printf|Command "echo"]]**


---

#### - ***Opções de depuração e rastreamento***

| Opção | Descrição                                                          |
| ----- | ------------------------------------------------------------------ |
| `-x`  | **Exibe cada comando** antes de executá-lo (debug).                |
| `-v`  | **Exibe cada linha** antes de executar, sem expansão de variáveis. |
| `-n`  | **Não executa** os comandos, apenas verifica a sintaxe.            |
**exemplos:***

```sh
bash -x script.sh
bash -n script.sh
```
<br>

---

#### - ***Opções de configuração do shell***

| Opção                   | Descrição                                            |
| ----------------------- | ---------------------------------------------------- |
| `--norc`                | Não lê os arquivos `~/.bashrc` ao iniciar.           |
| `--noediting`           | Desativa a edição de linha de comando (readline).    |
| `--posix`               | Força o shell a se comportar segundo o padrão POSIX. |
| `--login`               | Inicia como shell de login (idem a `-l`).            |
| `--noprofile`           | Ignora arquivos de profile de login                  |
| `--rcfile <arquivo>`    | Usa arquivo específico no lugar do `.bashrc`         |
| `--init-file <arquivo>` | Igual a `--rcfile`                                   |
***exemplos:***

```sh
bash --norc --noprofile
bash --rcfile ./meu_rc
```
<br>

---

#### - ***Outras opções úteis***

| Opção       | Descrição                         |
| ----------- | --------------------------------- |
| `--version` | Mostra a versão do Bash.          |
| `--help`    | Mostra ajuda resumida das opções. |
<br>

---

**Dicas de uso rápido:**

- **Executar comando simples:** `bash -c "echo Olá"`
- **Debug de script:** `bash -x script.sh`
- **Testar sintaxe sem executar:** `bash -n script.sh`
- **Ignorar customizações do usuário:** `bash --norc --noprofile`
- **Login interativo:** `bash -il`

---

#### ***Tabela completa de opção do bash***

| Opção              | Descrição                                                                | Exemplo                     |
| ------------------ | ------------------------------------------------------------------------ | --------------------------- |
| `-c string`        | Executa os comandos contidos em `string`.                                | `bash -c "echo Hello"`      |
| `-i`               | Força o shell a ser **interativo**.                                      | `bash -i`                   |
| `-l`, `--login`    | Inicia como **shell de login** (lê `/etc/profile` e `~/.bash_profile`).  | `bash -l`                   |
| `-s`               | Lê comandos da **entrada padrão (stdin)**.                               | `echo "echo Hello"          |
| `-x`               | Exibe cada comando antes de executá-lo (**debug**).                      | `bash -x script.sh`         |
| `-v`               | Exibe cada linha antes de executar (**verbose**).                        | `bash -v script.sh`         |
| `-n`               | Verifica a **sintaxe** do script sem executá-lo.                         | `bash -n script.sh`         |
| `--norc`           | Não lê o arquivo `~/.bashrc`.                                            | `bash --norc`               |
| `--noprofile`      | Não lê arquivos de profile de login (`/etc/profile`, `~/.bash_profile`). | `bash --noprofile`          |
| `--noediting`      | Desativa a **edição de linha de comando** (readline).                    | `bash --noediting`          |
| `--posix`          | Força o shell a se comportar segundo o **padrão POSIX**.                 | `bash --posix`              |
| `--rcfile file`    | Lê um arquivo específico em vez de `~/.bashrc`.                          | `bash --rcfile ./meu_rc`    |
| `--init-file file` | Igual a `--rcfile`, lê um arquivo de inicialização específico.           | `bash --init-file ./meu_rc` |
| `--version`        | Mostra a **versão** do Bash.                                             | `bash --version`            |
| `--help`           | Mostra ajuda resumida das opções.                                        | `bash --help`               |
| `--debugger`       | Inicia com suporte a **debugger** (usado com `bashdb`).                  | `bash --debugger script.sh` |
