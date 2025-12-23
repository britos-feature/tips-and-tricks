# **SSH** significa **Secure Shell** (casca segura).

É um **protocolo de rede** que permite **acesso remoto seguro** a computadores através de uma rede insegura (como a Internet). Ele substitui protocolos mais antigos e inseguros, como **Telnet** e **rlogin**.

O SSH foi criado para garantir:

- **Confidencialidade** → os dados transmitidos são criptografados.
- **Integridade** → os dados não podem ser alterados sem detecção.
- **Autenticidade** → você sabe com quem está se comunicando.


#### ***Casos de uso mais comuns:***

- Acessar um servidor Linux remotamente.
- Administrar servidores em data centers ou nuvem (AWS, Azure, Google Cloud).
- Transferir arquivos com segurança (via SCP ou SFTP).
- Encaminhar portas (port forwarding) com criptografia.
- Automatizar tarefas com scripts que usam SSH.


#### ***Casos mais avançados de uso com [[SSH_Avançado|SSH Advanced]]:***


- Upload (do seu computador → servidor):
- Download (do servidor → seu computador)
- Rsync (transferir arquivos ou sincronizar pastas inteiras via SSH)
- SFTP (SSH File Transfer Protocol) - Modo Interativo

#### ***Em resumo:***

| Ferramenta | Melhor uso                                          |
| ---------- | --------------------------------------------------- |
| `scp`      | Cópia simples de arquivos ou pastas                 |
| `rsync`    | Sincronização de pastas e transferência eficiente   |
| `sftp`     | Modo interativo para gerenciar arquivos remotamente |

## ***Funcionamento do SSH***

### Passo a passo básico:

1. **Cliente SSH** inicia uma conexão com o **Servidor SSH**.
2. O servidor envia sua **chave pública** para o cliente.
3. O cliente verifica a chave pública do servidor (para garantir que é o servidor certo).
4. O cliente e o servidor negociam uma **chave de sessão criptografada**.
5. O cliente se **autentica** (senha ou chave privada).
6. Após autenticação, a comunicação é segura e criptografada.

### ***Componentes:***

- **Servidor SSH** → software que "ouve" requisições (geralmente na porta **22**).
- **Cliente SSH** → software usado para se conectar ao servidor.

### Programas típicos:

- Servidor: `sshd` (OpenSSH daemon).
- Cliente: `ssh` (linha de comando), `PuTTY` (Windows), `MobaXterm`, `Termius`.

### ***Métodos de Autenticação***

#### a) **Senha**

- Cliente digita uma senha.
- É simples, mas menos seguro.
    

#### b) **Chave pública/privada (recomendado)**

- Cliente possui um **par de chaves**:
    
    - **Chave privada** → guardada localmente, secreta.
    - **Chave pública** → copiada para o servidor.

> Durante a conexão, o cliente prova que tem a chave privada correspondente à chave pública autorizada no servidor.


#### Benefícios:

- Muito mais seguro que senha.
- Permite automação sem digitar senha (usando `ssh-agent`).


### ***Criptografia no SSH***

SSH utiliza várias técnicas de criptografia:

- **Criptografia simétrica** (ex: AES) → para proteger a sessão.
- **Criptografia assimétrica** (ex: RSA, ECDSA, Ed25519) → usada na autenticação e na troca inicial de chaves.
- **Funções de hash** (ex: SHA-2) → para garantir integridade dos dados.


### ***Estrutura dos arquivos de configuração***

#### No cliente:

- `~/.ssh/config` → configurações personalizadas de host.
- `~/.ssh/id_rsa` ou `~/.ssh/id_ed25519` → chave privada.
- `~/.ssh/id_rsa.pub` ou `~/.ssh/id_ed25519.pub` → chave pública.

#### No servidor:

- `/etc/ssh/sshd_config` → configurações do daemon SSH.
- `~/.ssh/authorized_keys` → lista de chaves públicas autorizadas a se conectar.

---

## ***Instalação SSH***

- **Cliente SSH** → para se conectar a servidores (ex: seu computador pessoal).
- **Servidor SSH** → para receber conexões (ex: em um servidor Linux na nuvem).

### ***Instalar Cliente SSH***
 **Linux (Ubuntu, Debian, Fedora, Arch, etc)**

Normalmente o **cliente SSH** (`ssh`) já vem instalado.
Para garatir ou instalar:

```shell
# Debian/Ubuntu
sudo apt update
sudo apt install openssh-client

# Fedora
sudo dnf install openssh-clients

# Arch
sudo pacman -S openssh
```


> Alternativas de cliente:
> 	-  **PuTTY** (cliente SSH gráfico para Windows)
> 	-  **MobaXterm** (cliente SSH avançado com recursos extras)


## ***Instalar Servidor SSH***

O **servidor SSH** é o que você instala em um computador que você quer acessar remotamente (ex: VPS, servidor Linux).

**Linux** (**Ubuntu / Debian**)

```shell
sudo apt update
sudo apt install openssh-server
```

-  Verificar se o serviço está ativo:

```shell
sudo systemctl status ssh
```

- Iniciar e habilitar o servidor:

```shell
sudo systemctl enable ssh
sudo systemctl start ssh
```


### ***Testar a instalação***

- Para testar o **cliente**:

```shell
ssh usuario@ip_do_servidor
```

- Para testar se o **servidor** está aceitando conexões:

```shell
sudo systemctl status ssh   # ou sshd
```


## ***Exemplos práticos de uso***

- Conectar a um servidor:

```shell
ssh usuario@ip_do_servidor
```

- Copiar arquivo via SCP:

```shell
scp arquivo.txt usuario@ip_do_servidor:/caminho/destino/
```

- Usar um túnel (port forwarding):

```shell
ssh -L 8080:servidor_destino:80 usuario@ip_do_servidor
# Faz com que localhost:8080 redirecione tráfego para a porta 80 do servidor.
```

####  **Segurança e boas práticas**

-> Use **chaves públicas/privadas** ao invés de senhas.  
-> Desative login por senha no servidor (`PasswordAuthentication no`).  
->  Use um **firewall** para limitar IPs que podem acessar a porta 22.  
->  Mude a porta padrão (opcional, mas ajuda contra ataques automatizados).  
->  Use um agente SSH (`ssh-agent`) para gerenciar suas chaves privadas.

### ***Resumo final***

| Protocolo    | O que faz            | Segurança                                    |
| ------------ | -------------------- | -------------------------------------------- |
| SSH          | Acesso remoto seguro | Criptografado, autenticado                   |
| cliente SSH  | `openssh-client`     | Já vem em macOS e Linux, opcional em Windows |
| servidor SSH | `opne-ssh-server     | Instala manualmente no Linux/Windows         |


### ***Gerando um par de chaves SSH***

Útil para  evita digitar senha toda vez, muito mais seguro que senha e permite automação.
    
***Funcionamento***:
- Você gera **duas chaves**:
	-  **Chave privada** → fica só com você (protegida).
	-  **Chave pública** → você envia para o servidor (fica em `~/.ssh/authorized_keys`).


#### Gerar as chaves (no seu computador local):

```shell
	ssh-keygen -t ed25519 -C "seuemail@example.com"

# -t ed25519 → tipo de chave moderna e segura (recomendado). Alternativa: `rsa` (antigo).
# -C → comentário para identificar a chave.
```

### ***Copiar a chave pública para o servidor:***

```shell
ssh-copy-id usuario@ip_do_servidor
```

Ou manualmente, copiando o conteúdo e colando em **`~/.ssh/authorized_keys`** no servidor.

---

## ***Configurarndo um servidor SSH seguro***

- Instalar o servidor:

```shell
sudo apt install openssh-server    # Ubuntu/Debian
sudo systemctl enable ssh
sudo systemctl start ssh
```


- Editar a configuração do SSH:

```shell
sudo nano /etc/ssh/sshd_config
```


- Configurações importantes:

```text
# Proteger contra ataques automatizados
PermitRootLogin no            # NÃO permitir login como root direto
PasswordAuthentication no     # NÃO permitir login com senha (só chave)
PermitEmptyPasswords no
ChallengeResponseAuthentication no
```


-  Porta personalizada (opcional):

```shell
Port 2222    # Exemplo: porta alternativa (diminui bots automáticos)
```


- ⚠️ Se você mudar a porta, precisa abrir no firewall:

```shell
sudo ufw allow 2222/tcp
```


- Verificações:

```shell
sudo systemctl restart ssh # restart serviço
sudo systemctl status ssh # status do serviço
ssh -p 2222 usuario@ip_do_servidor # testar conexão
```


---

## ***Automatizar tarefas com SSH***

- Executar um comando remoto:

```shell
ssh usuario@ip_do_servidor 'uptime'
```

- Copiar um arquivo via script:

```shell
scp arquivo.txt usuario@ip_do_servidor:/caminho/destino/
```

- Script simples de deploy (exemplo):

```shell
#!/bin/bash

# Deploy de app para o servidor

echo "Iniciando deploy..."

ssh usuario@ip_do_servidor << 'EOF'
cd /var/www/meuapp
git pull origin main
systemctl restart meuapp.service
EOF

echo "Deploy finalizado!"
```


#### ***Dica: usar **SSH Agent** para evitar digitar senha da chave privada toda vez:***

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```


