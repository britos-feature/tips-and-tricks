O comando **`git remote`** serve para **gerenciar os repositórios remotos** associados ao seu repositório local.  Em outras palavras:
	Ele conecta seu projeto local (no seu computador) com um servidor remoto 
	por exemplo, (**GitHub**, **GitLab**, **Bitbucket** etc.).

---

## Como funciona

Quando você cria um repositório local com:

```bash
git init
```

ele ainda **não sabe** onde enviar (push) ou buscar (pull) código.  
Você precisa **conectar** um repositório remoto:

```bash
git remote add origin https://github.com/usuario/repositorio.git
```

Aqui:

- `origin` → é o **nome do remoto** (apelido).  
    Você pode chamar de outro nome se quiser, mas “origin” é o padrão.
    
- O link é o **endereço do repositório remoto**.
    

---

## Comandos mais usados

### 🔍 Ver remotos configurados

```bash
git remote -v
```

Mostra todos os remotos e seus endereços:

```
origin  https://github.com/usuario/repositorio.git (fetch)
origin  https://github.com/usuario/repositorio.git (push)
```

---

### Adicionar remoto

```bash
git remote add nome_remoto URL
```

Exemplo:

```bash
git remote add origin https://github.com/britos/meu-projeto.git
```

---

### Alterar o URL de um remoto

Se você quiser mudar o repositório remoto (por exemplo, trocou o nome ou passou de HTTPS para SSH):

```bash
git remote set-url origin git@github.com:britos/meu-projeto.git
```

---

###  Remover um remoto

```bash
git remote remove origin
```

---

### Ter mais de um remoto

Sim! 😎  
Você pode ter vários remotos (por exemplo, um no GitHub e outro no GitLab):

```bash
git remote add github https://github.com/britos/meu-projeto.git
git remote add gitlab https://gitlab.com/britos/meu-projeto.git
```

E enviar para cada um separadamente:

```bash
git push github main
git push gitlab main
```

---

## Fluxo comum

1. Criar repo local:
    
    ```bash
    git init
    ```
    
2. Adicionar remoto:
    
    ```bash
    git remote add origin https://github.com/britos/meu-projeto.git
    ```
    
3. Enviar o código:
    
    ```bash
    git push -u origin main
    ```
    

---

## Repositórios Públicos vs Privados

### **Público**

- Qualquer pessoa pode **ver e clonar** o repositório:

```bash
git clone https://github.com/usuario/repositorio.git
```

 > Mas **ninguém pode enviar (push)** alterações — apenas **quem tem permissão de escrita**.

**Exemplo:**
	- Você publica um projeto open source.
	- Outros podem clonar, mas não podem alterar o original sem sua autorização.


---

### **Privado**

- Apenas quem **você autoriza** pode **ver ou clonar** o repositório.
- Também só os autorizados podem **fazer push**.

---

## Permissões de acesso

Em qualquer repositório (público ou privado), **só quem tem permissão** pode enviar (push) alterações. No GitHub, há três papéis principais:

| Papel     | Pode ver | Pode fazer push | Pode gerenciar colaboradores |
| --------- | -------- | --------------- | ---------------------------- |
| **Read**  | ✅        | ❌               | ❌                            |
| **Write** | ✅        | ✅               | ❌                            |
| **Admin** | ✅        | ✅               | ✅                            |

Você gerencia isso em:

> ⚙️ _Settings → Collaborators and teams_

Ou adicionando diretamente um colaborador:

- Vá em **Settings** > **Collaborators**
- Clique em **“Add people”**
- Digite o nome do usuário do GitHub

---

## Autenticação

Mesmo com acesso, o usuário precisa se **autenticar**:

- via **HTTPS** (com token pessoal — PAT)
- ou via **SSH** (com chave configurada)

Assim o GitHub garante que quem faz push realmente é o dono da conta.

---

## Resumo rápido

| Ação              | Público                   | Privado                   |
| ----------------- | ------------------------- | ------------------------- |
| Clonar            | ✅ Qualquer um             | ❌ Apenas convidados       |
| Fazer push        | ❌ Apenas dono/colaborador | ❌ Apenas dono/colaborador |
| Ver código online | ✅ Sim                     | ❌ Apenas convidados       |

---

