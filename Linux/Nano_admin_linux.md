# Manual do Editor Nano para Administradores Linux

Autor: Guia prático para administração de servidores Linux Formato:
Markdown

------------------------------------------------------------------------

# 1. Introdução

O **Nano** é um editor de texto em modo terminal muito utilizado em
servidores Linux. Ele é simples, leve e geralmente já vem instalado em
distribuições como:

-   Ubuntu Server
-   Debian
-   Rocky Linux
-   AlmaLinux
-   Fedora

Administradores utilizam o Nano para editar rapidamente arquivos de
configuração do sistema.

------------------------------------------------------------------------

# 2. Abrindo Arquivos

Abrir um arquivo existente:

``` bash
nano arquivo.txt
```

Criar um novo arquivo:

``` bash
nano novo_arquivo.txt
```

Editar arquivo do sistema:

``` bash
sudo nano /etc/ssh/sshd_config
```

Abrir arquivo já em uma linha específica:

``` bash
nano +120 arquivo.txt
```

------------------------------------------------------------------------

# 3. Interface do Nano

A interface possui três partes principais:

1.  Área de edição
2.  Barra de mensagens
3.  Barra de atalhos

Exemplo:

    ^G Help   ^O WriteOut   ^W Where Is   ^K Cut
    ^X Exit   ^R Read File  ^\ Replace

O símbolo **\^** significa **CTRL**.

------------------------------------------------------------------------

# 4. Navegação no Arquivo

  Tecla      Função
  ---------- ------------------------
  ↑ ↓        mover entre linhas
  ← →        mover entre caracteres
  Ctrl + A   início da linha
  Ctrl + E   final da linha
  Ctrl + Y   página acima
  Ctrl + V   página abaixo
  Alt + \\   início do arquivo
  Alt + /    final do arquivo

------------------------------------------------------------------------

# 5. Edição de Texto

  Tecla      Função
  ---------- ------------------
  Ctrl + K   recortar linha
  Ctrl + U   colar
  Alt + 6    copiar
  Ctrl + D   apagar caractere
  Ctrl + H   backspace
  Ctrl + J   justificar texto

------------------------------------------------------------------------

# 6. Seleção de Texto

Iniciar marcação:

    Ctrl + ^

Mover o cursor para selecionar.

Copiar seleção:

    Alt + 6

------------------------------------------------------------------------

# 7. Buscar Texto

Buscar palavra:

    Ctrl + W

Buscar novamente:

    Alt + W

------------------------------------------------------------------------

# 8. Substituir Texto

Substituir palavras:

    Ctrl + \

O Nano solicitará:

1.  palavra a procurar
2.  palavra de substituição

------------------------------------------------------------------------

# 9. Navegar para Linha Específica

    Ctrl + _

Digite o número da linha.

Exemplo:

    150

------------------------------------------------------------------------

# 10. Salvar Arquivos

Salvar:

    Ctrl + O

Confirmar:

    Enter

------------------------------------------------------------------------

# 11. Sair do Nano

    Ctrl + X

Se houver alterações não salvas, o Nano perguntará se deseja salvar.

------------------------------------------------------------------------

# 12. Desfazer e Refazer

  Tecla     Função
  --------- ----------
  Alt + U   desfazer
  Alt + E   refazer

------------------------------------------------------------------------

# 13. Inserir Conteúdo de Outro Arquivo

    Ctrl + R

Digite o caminho do arquivo.

Exemplo:

    /etc/hosts

------------------------------------------------------------------------

# 14. Trabalhando com Vários Arquivos

Abrir múltiplos arquivos:

``` bash
nano arquivo1.txt arquivo2.txt
```

Trocar entre arquivos:

    Alt + >
    Alt + <

------------------------------------------------------------------------

# 15. Recursos Avançados

Mostrar numeração de linhas:

``` bash
nano -l arquivo.txt
```

Abrir em modo somente leitura:

``` bash
nano -v arquivo.txt
```

Backup automático:

``` bash
nano -B arquivo.txt
```

------------------------------------------------------------------------

# 16. Arquivos Comuns Editados por Administradores

### Configuração SSH

    /etc/ssh/sshd_config

### Configuração de rede

    /etc/netplan/*.yaml

### Hosts locais

    /etc/hosts

### Samba

    /etc/samba/smb.conf

### Fstab (montagem de discos)

    /etc/fstab

------------------------------------------------------------------------

# 17. Boas Práticas

✔ Sempre faça backup antes de alterar arquivos críticos

Exemplo:

``` bash
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp
```

✔ Teste configuração antes de reiniciar serviços

✔ Utilize comentários nos arquivos

Exemplo:

    # Configuração alterada para acesso interno

------------------------------------------------------------------------

# 18. Exemplo Real de Administração

Editar configuração do Samba:

``` bash
sudo nano /etc/samba/smb.conf
```

Após editar:

``` bash
testparm
sudo systemctl restart smbd
```

------------------------------------------------------------------------

# 19. Dicas de Produtividade

Mostrar posição do cursor:

    Ctrl + C

Ir para próxima ocorrência de busca:

    Alt + W

Cancelar operação:

    Ctrl + C

------------------------------------------------------------------------

# Conclusão

O **Nano** é uma ferramenta essencial para administradores Linux que
trabalham em servidores via **SSH**.

Dominar seus atalhos permite:

-   editar arquivos rapidamente
-   solucionar problemas
-   administrar servidores sem interface gráfica

Ele é um dos primeiros editores que todo administrador Linux deve
dominar.
