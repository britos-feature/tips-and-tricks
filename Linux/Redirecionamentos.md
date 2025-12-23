## ***Redirecionamentos de saídas  Linux***

Redirecionamento de saída é o mecanismo usado para enviar o resultado de um comando para outro lugar que **não** seja a tela padrão **( stdout )** ou para tratar erros **( stderr )**.

***Entendimento de Redirecionamento.***
	
No Unix, **tudo é arquivo** – inclusive entrada, saída e erros. Cada processo tem 3 canais principais (descritores de arquivo):

- **0 – stdin** → entrada padrão (normalmente o teclado)
- **1 – stdout** → saída padrão (normalmente a tela)
- **2 – stderr** → saída de erros (normalmente a tela)

### Redirecionamento de saída padrão (stdout — file descriptor 1)

- **Enviar saída para um arquivo** (sobrescrevendo)
`ls > list.txt`
	-  Cria (ou substitui) o arquivo `list.txt` com a saída do comando

- **Enviar saída para um arquivo** (acrescentando no final) **appendices**
`ls >> lista.txt
	- Adiciona ao final do arquivo `list.txt` sem apagar o conteúdo anterior.


### Redirecionamento de erro padrão (stderr — file descriptor 2) 

- **Salvar apenas erros em um arquivo**
`ls /pasta_inexistente 2> erros.txt`

- **Acrescentar erros ao arquivo**
`ls /pasta_inexistente 2>> erros.txt`


### Redirecionando saída e erros juntos

- **Enviar ambos para o mesmo arquivo**
`comando > saida_e_erros.txt 2>&1`

- **Enviar ambos para o mesmo arquivo utilizando sintaxe mais moderna (bash 4+)**
`comando &> saida_e_erros.txt`

- **Acrescentar ambos ao mesmo arquivo**
`comando >> saida_e_erros.txt 2>&1`

- **Acrescentar ambos ao mesmo arquivo utilizando sintaxe mais moderna (bash 4+)**
`comando &>> saida_e_erros.txt 


### Jogar a saída fora (redirecionar para o “buraco negro”)
***/dev/null***

- **Ignorar apenas a saída padrão**
`comando > /dev/null`

- **Ignorar apenas os erros**
`comandos 2>/dev/null`

- **Ignorar tudo (saída e erros)**
`comando > /dev/null 2>&1`

- **Fechar descritor _`stderror`_ (2>&-)** 
`comando 2>&-`

***Resumindo:***
- **`2>/dev/null`** → erros são descartados, mas de forma “limpa” (**stderr** continua existindo).
- **`2>&-`** → stderr é fechado; se o programa tentar escrever nele, pode receber erro de sistema<br> (“**Bad file descriptor**”).
<br>
### **Leitura de entrada via redirecionamento**

- **Redirecionar entrada** (stdin — file descriptor 0)
`wc -l < arquivo.txt`

> Conta as linhas de `arquivo.txt` sem usar `cat`


### Operador `<<` **Here-doc** (_Here Document_), 
**Here-doc (<<)** serve para enviar múltiplas linhas de texto como entrada ( **STDIN** ) para um comando, até encontrar um delimitador específico.

**Algumas utilidades para** ***HERE-DOC***

1. Criar arquivos de configuração

```shell
cat << EOF > config.ini
[server]
host=localhost
port=8080
EOF
```

2. Mensagens multi-linha em scripts

```shell
cat << EOF
Bem-vindo ao instalador!
Este script irá configurar o sistema.
Pressione Ctrl+C para cancelar.
EOF
```

3. Enviar comandos para clientes remotos (ssh)

```shell
ssh usuario@servidor << EOF
cd /var/www
ls -l
exit
EOF
```

4. Inserir dados no banco de dados

```shell
mysql -u root -p << EOF
CREATE DATABASE teste;
USE teste;
CREATE TABLE users (id INT, nome VARCHAR(50));
EOF
```

5. Gerar scripts dinâmicos

```shell
cat << EOF > script.sh
#!/bin/bash
echo "Este script foi gerado automaticamente"
EOF

chmod +x script.sh
```

6. Documentação inline no script

```shell
: << 'DOC'
Este bloco serve apenas como comentário gigante.
Pode conter múltiplas linhas.
Não será executado.
DOC
```

7. Criar e-mails com corpo

```shell
sendmail user@example.com << EOF
Subject: Aviso importante
From: admin@example.com
Olá, este é um e-mail enviado via script.
EOF
```


**Resumo rápido**

|Operador|Uso|Característica|
|---|---|---|
|`<<`|Heredoc|Bloco de múltiplas linhas como entrada|
|`<<-`|Heredoc com _tabs_ removidas|Útil para manter indentação no script|
|`<<<`|Herestring|Entrada de uma única linha/string|

**Resumo das vantagens do Heredoc**

- Evita múltiplos `echo` ou `printf`
- Não precisa criar arquivos temporários para mandar dados
- Permite manter identação com `<<-`
- Aceita variáveis, ou evita expansão usando `'EOF'`


### Redirecionamentos ***especiais*** ( |  pipe, tee)

***| (pipe)***, passa a saída de um comando para a entrada de outro (esse método utiliza um **sub-shell**).
***tee***, passa a saída de um comando para a saída padrão (tela) e também para um arquivo indicado.

`ls D* | tee list_ls.txt`

