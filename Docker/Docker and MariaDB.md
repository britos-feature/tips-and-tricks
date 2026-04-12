
# 🐳 Docker  — Guia prático

Este tutorial em **Markdown (.md)** foi criado para usuários **Ubuntu** (especialmente Ubuntu GNOME) e mistura **conceitos para iniciantes** com **boas práticas e recursos avançados**, servindo tanto para aprendizado quanto para consulta profissional.

---

## 🎯 Para quem é este guia?

- 👶 Iniciantes que nunca usaram Docker
- 🧑‍💻 Desenvolvedores que já usam Docker mas querem organizar melhor
- ⚙️ Usuários Linux/Ubuntu que rodam containers localmente

---

## 📌 Docker - (explicação)

Docker é uma plataforma que permite executar aplicações dentro de **containers**, que são ambientes isolados, leves e reproduzíveis.

Você empacota tudo que a aplicação precisa:

- Código
- Dependências
- Bibliotecas
- Variáveis de ambiente

👉 _Funciona na sua máquina, no servidor e na cloud da mesma forma._

---

## 🧱 Conceitos Fundamentais

### 🖼️ Imagem 

Uma **imagem Docker** é um modelo imutável usado para criar containers.

```bash
docker images
```

---

### 📦 Container 

Um **container** é uma imagem em execução.

```bash
docker ps        # containers rodando
docker ps -a     # todos
```

---

### 🧾 Dockerfile 

Arquivo que define passo a passo como uma imagem será criada.

---

### 💾 Volume 

Volumes garantem **persistência de dados**, mesmo se o container for removido.

---

### 🌐 Network 

Permite comunicação segura entre containers (ex: app ↔ banco).

---

## 🛠️ Instalação do Docker no Ubuntu

### Método recomendado (repositório oficial Ubuntu)

```bash
sudo apt update
sudo apt install docker.io -y
```

Iniciar e habilitar no boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

### Método manual Debian ( oficial  dockerDocs)

```bash

# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

### Método manual Ubuntu ( oficial dockerDocs )

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

---

### 👤 Rodar Docker sem sudo (Essencial)

```bash
sudo usermod -aG docker $USER
```

🔁 **Reinicie a sessão** para aplicar.

Teste:

```bash
docker run hello-world
```

---

## ▶️ Primeiros Containers (Iniciante)

### Container simples

```bash
docker run ubuntu echo "Olá Docker"
```

### Container interativo

```bash
docker run -it ubuntu bash
```

### Container em background

```bash
docker run -d nginx
```

---

## 🔁 Gerenciamento de Containers

```bash
docker stop <container>
docker start <container>
docker restart <container>
docker rm <container>
```

Remover containers parados:

```bash
docker container prune
```

---

## 🗑️ Gerenciamento de Imagens

```bash
docker images
docker rmi <imagem>
docker image prune
```

---

## 🔌 Portas (Muito Importante)

```bash
docker run -d -p 8080:80 nginx
```

📌 **Sintaxe**

```
-p PORTA_HOST:PORTA_CONTAINER
```

Verificar portas expostas:

```bash
docker ps
docker port <container>
```

---

## 💾 Volumes — Persistência Real

### Volume nomeado (Recomendado)

```bash
docker volume create dados
```

```bash
docker run -d -v dados:/var/lib/mysql mariadb
```

### Bind mount (Avançado)

```bash
docker run -d -v $(pwd)/data:/app/data node
```

📌 Use **bind mount** para desenvolvimento e **volumes** para produção.

---

## 🔄 Restart Automático (Produção)

```bash
docker run -d --restart unless-stopped nginx
```

Políticas:

- `no`
- `always`
- `unless-stopped` ✅
- `on-failure`

---

## 🔐 Variáveis de Ambiente

```bash
docker run -e MYSQL_ROOT_PASSWORD=123456 mysql
```

Arquivo `.env`:

```bash
docker run --env-file .env mysql
```

---

## 🧑‍🍳 Dockerfile — Exemplo Real

```Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

Build:

```bash
docker build -t minha-app .
```

Run:

```bash
docker run -d -p 3000:3000 minha-app
```

---

## 🧩 Docker Compose (Essencial)

Arquivo `docker-compose.yml`:

```yaml
version: "3.9"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    env_file:
      - .env
    restart: unless-stopped

  db:
    image: mariadb:11
    environment:
      MARIADB_ROOT_PASSWORD: root
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  db_data:
```

Executar:

```bash
docker compose up -d
```

Parar e remover:

```bash
docker compose down
```

---

## 🔍 Debug e Inspeção (Avançado)

```bash
docker logs <container>
docker inspect <container>
docker exec -it <container> bash
```

Monitorar consumo:

```bash
docker stats
```

---

## 🧹 Limpeza (Manutenção)

```bash
docker system prune -a
```

⚠️ Remove **tudo que não está em uso**.

---

## ✅ Boas Práticas Profissionais

- Use imagens **alpine** quando possível
- Nunca exponha senhas no Dockerfile
- Use `.env`
- Um processo por container
- Use Docker Compose para stacks
- Evite `latest` em produção

---

## 📚 Referências

- Docker Docs
- Docker Hub
- Docker Compose

---


# 🗄️ Maria DB


## Docker Run (Simples e direto)

### 📌 Quando usar?

- Testes rápidos
- Ambiente local
- Aprendizado inicial

### 📦 Comando básico

```bash
docker run -d \
  --name mariadb \
  -e MARIADB_ROOT_PASSWORD=senha_forte \
  -p 3306:3306 \
  mariadb:latest
```

### 🔍 O que cada parte faz?

| Opção                   | Explicação             |
| ----------------------- | ---------------------- |
| `-d`                    | Executa em background  |
| `--name mariadb`        | Nome do container      |
| `-e`                    | Variável de ambiente   |
| `MARIADB_ROOT_PASSWORD` | Senha do root          |
| `-p 3306:3306`          | Porta host → container |
| `mariadb:latest`        | Imagem oficial         |

### ❌ Problema desse método

Se o container for removido:

```bash
docker rm -f mariadb
```

👉 **TODOS os dados são perdidos**

---

## Docker Run com volume (Persistência de dados) ✅

### 📌 Quando usar?

- Desenvolvimento sério
- Produção simples
- Evitar perda de dados

### 📦 Comando recomendado

```bash
docker run -d \
  --name mariadb \
  --restart unless-stopped \
  -e MARIADB_ROOT_PASSWORD=senha_forte \
  -v mariadb_data:/var/lib/mysql \
  -p 3306:3306 \
  mariadb:11
```

### 📁 O que é esse volume?

```bash
-v mariadb_data:/var/lib/mysql
```

- `mariadb_data` → volume Docker
- `/var/lib/mysql` → diretório onde o MariaDB guarda os dados

📌 **Mesmo removendo o container**, os dados permanecem.

### 🔄 Política de reinício

```bash
--restart unless-stopped
```

| Opção            | Comportamento                             |
| ---------------- | ----------------------------------------- |
| `no`             | Nunca reinicia                            |
| `always`         | Sempre reinicia                           |
| `unless-stopped` | Reinicia exceto se você parar manualmente |

✅ **Recomendado para produção**

---

## Docker Run com bind mount (Volume manual)

### 📌 Quando usar?

- Precisa acessar arquivos diretamente
- Backup manual
- Debug

### 📦 Exemplo

```bash
docker run -d \
  --name mariadb \
  -e MARIADB_ROOT_PASSWORD=senha_forte \
  -v /opt/mariadb:/var/lib/mysql \
  -p 3306:3306 \
  mariadb
```

### ⚠️ Atenção

- Permissões podem quebrar o container
- Usuário interno do MariaDB ≠ usuário do host

📌 **Menos recomendado** que volumes Docker.

---

## Docker Compose (Profissional e escalável) ⭐⭐⭐

### 📌 Quando usar?

- Ambientes reais
- Múltiplos serviços (API + DB)
- Versionamento em Git

### 📄 `docker-compose.yml`

```yaml
services:
  mariadb:
    image: mariadb:11
    container_name: mariadb
    restart: unless-stopped
    ports:
      - "3306:3306"
    environment:
      MARIADB_ROOT_PASSWORD: senha_forte
      MARIADB_DATABASE: appdb
      MARIADB_USER: appuser
      MARIADB_PASSWORD: apppass
    volumes:
      - mariadb_data:/var/lib/mysql

volumes:
  mariadb_data:
```

### ▶️ Subir o serviço

```bash
docker compose up -d
```

### 🛑 Parar sem perder dados

```bash
docker compose down
```

📌 **Volume continua intacto**

---

## Rede Docker + isolamento (Avançado)

### 📌 Quando usar?

- Segurança
- Microserviços
- DB não exposto externamente

### 🌐 Criar rede

```bash
docker network create backend
```

### MariaDB sem expor porta

```bash
docker run -d \
  --name mariadb \
  --network backend \
  -e MARIADB_ROOT_PASSWORD=senha \
  -v mariadb_data:/var/lib/mysql \
  mariadb
```

### App acessa via hostname

```text
mariadb:3306
```

🔐 **Mais seguro que `-p 3306:3306`**

---

## Verificações importantes

### 🔍 Ver se está rodando

```bash
docker ps
```

### 🔍 Ver portas expostas

```bash
docker port mariadb
```

### 🔍 Logs

```bash
docker logs mariadb
```

### 🔍 Entrar no banco

```bash
docker exec -it mariadb mariadb -u root -p
```

---
## Boas práticas finais ✅

✔️ Sempre use **volume persistente**  
✔️ Evite `latest` em produção  
✔️ Use `restart unless-stopped`  
✔️ Prefira **Docker Compose**  
✔️ Não exponha porta do DB sem necessidade  
✔️ Nunca use root na aplicação

---
