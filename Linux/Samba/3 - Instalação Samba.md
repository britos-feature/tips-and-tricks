# Instalação via Source Code (código fonte)
**SAMBA**  - Instalação via ***source code repository***

- ##### Obtendo _source code repository_
	
	**`wget -c https://download.samba.org/pub/samba/stable/samba-4.23.2.tar.gz`**

---

- ##### Descompactação do arquivo baixado / descompactando _source code_
	
	**`tar xf samba-4.23.2.tar.gz`**

---

- ## Instalação das dependências
	Dependências(arquivos) necessárias para instalação do source code
	(via script pronto no diretório SAMBA descompactado)
	
	**`./bootstrap/generated-dists/ubuntu2404/bootstrap.sh`**

---

- ##### Desinstalando  packages desnecessários instalados com o `script` (dependências)
	
	**`apt-get purge krb5-kdc landscape-common`**
	**`apt-get --purge autoremove -y`**

---

- ##### Ajuste **COMPILAÇÃO** (`./configure`)

	- **Prepara o programa** para ser instalado no seu sistema
	- **Verifica dependências** e configura o ambiente
	- **Gera o `Makefile`** para compilação
	
	**`./configure --with-systemd --prefix=/usr/local/samba --enable-fhs**

	**Explicação:**
	- **`--with-systemd`** = Inicializar o serviço ao systemd
	- **`--prefix=/...`** = local para instalação do samba
	- **`--enable-fhs`** = padrão que define **como os diretórios do Linux devem ser organizados** ou em outras palavras: O FHS padroniza onde cada tipo de arquivo deve ficar no sistema.

---

- #####  Compila source code (código fonte)
	
	**`make`**

	- **Processo:**
		- lê o `Makefile`
		- compila os arquivos (`.c`, `.cpp`)
		- gera binários (executáveis)

	**Resumo:** transformação de código em programa real

---
	
- #### **Instala a aplicação no sistema**
	
	**`make install`**

	**- Processo:**
		- copia os binários para diretórios padrão:
			- **`/usr/local/bin`**
			- **`/usr/local/sbin`**
			- instala configs, libs, manuais

---

### Exportando os arquivo binários ao PATH (default user)
Method para exportar arquivos binários para a variável **PATH** do usuário

- ### Opções para localizar os binários de um comando

1. Mostra **qual executável será usado**
	
	**`which [command]`
	`which ls`

> Simple e direto - Usa no **PATH**

---

2. Modo mais confiável em scripts - **command**
	
	**`command -v ls`**

> Melhor prática para scripts  
   Funciona com aliases e funções também

---

3. Mostra **o que exatamente é o comando** - **type**

	**`type ls`**

> Útil para identificar:  `binário`, `alias` e `functions`

---

4.  Verifica / Procura em mais lugares **(não só no PATH)** 

	**`whereis ls`**

> Mostra (binário, documentação, código fonte (se houver))


5. Mostras todos no PATH

	**`type -a python`**

> Mostra **todas as ocorrências no PATH**, na ordem de prioridade.



6. Resuno do comando**: 

| Comando      | Função                              |
| ------------ | ----------------------------------- |
| `which`      | Mostra o executável                 |
| `command -v` | Melhor para scripts                 |
| `type`       | Diz o tipo (alias, binário, função) |
| `type -a`    | Mostra todos no PATH                |
| `whereis`    | Busca mais amplo                    |

---

- ### **Opções para incluir binários ao PATH do usuário

1.  Forma mais recomendada **(padrão Linux)**

	 - Colocar o binário em um diretório **global** do **PATH**

		Os diretórios padrão globais são:
			
			- /usr/local/bin  
			- /usr/local/sbin 
			- /usr/bin  
			- /usr/sbin

##### Exemplo:

**`sudo cp /usr/local/samba/bin/samaba-tool /usr/local/bin/`**
**`sudo chmod +x /usr/local/bin/samaba-tool`**

> **Resultado:**  Todos os usuários conseguem executar **`samba-tool`**

Essa é a abordagem **mais limpa e padrão** possível

---

2.  Criar um diretório próprio e adicionar ao PATH global

	- Organização melhor (ex: `/opt/meus-binarios`):

		- **Criar diretório**
			**`mkdir -p /opt/meus-binarios`**

---

3. Copiar binários:

	**`cp /usr/local/samba/bin/samba-tool /opt/my-apps/`**  
	**`chmod +x /opt/meus-binarios/my-apps`**

---

# ******OPÇÃO UTILIZADA (`profile.d`)******

4. Adicionar ao **PATH** para TODOS usuários

	- Crie um arquivo **`samba.sh`** em **`/etc/profile.d/`**:

		**`/etc/profile.d/samba.sh`**

	- Conteúdo do arquivo **`samba.sh`**

		**`export PATH="/usr/local/samba/bin:/usr/local/samba/sbin:$PATH"`**


> Aplicação do arquivo criado:   **`source /etc/profile.d/samba.sh`** ou faça **Logout/login**
   Agora todos usuários terão acesso. 


---

5. Alternativa mais “forte” **(para TODOS ambientes)**

	- Editar diretamente: **`/etc/environment`**


**Exemplo:**
	**`PATH="/opt/my-apps:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

**Vantagem:**
- Afeta todos os usuários
- Funciona em mais cenários (login gráfico, serviços, etc.)


⚠️ Cuidado:
	- Não usa **`export`**
	- Sobrescreve **PATH** (tem que incluir tudo)


# Para root também (caso especial)

- **Se quiser garantir para root edit esses arquivos:**

	**`/root/.bashrc`** 	ou 	**`/root/.profile`**

---

## Boas práticas

✔️ Use **`/usr/local/bin`** para binários manuais  
✔️ Use **`/opt`** para aplicações customizadas  
✔️ Evite alterar **`/usr/bin`** (gerenciado pelo sistema)

---

# Resumo rápido

| Método             | Quando usar                      |
| ------------------ | -------------------------------- |
| `/usr/local/bin`   | Melhor opção (simples e padrão)  |
| `/etc/profile.d`   | Customização organizada          |
| `/etc/environment` | Forçar global em todos contextos |
| `/opt + PATH`      | Projetos organizados             |

---

