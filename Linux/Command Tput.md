O **`tput`** é um comando do Unix/Linux usado para **controlar a formatação e o comportamento do terminal**.  
Ele acessa a base de dados `terminfo` (que descreve as capacidades do terminal) e envia sequências de escape apropriadas para realizar ações como:

- Mover o cursor
- Limpar a tela
- Mudar cor do texto e do fundo
- Ativar/desativar atributos (negrito, sublinhado, etc.)
- Consultar informações sobre o terminal (largura, altura)

## ***Opções mais úteis e usadas no dia a dia***

### 🔹 _**Controle do terminal**_

- `tput clear` → limpa a tela (igual ao `clear`).
- `tput reset` → reseta o terminal (limpa + restaura configuração).
- `tput civis` → esconde o cursor.
- `tput cnorm` → mostra o cursor de volta.
- `tput cup <linha> <coluna>` → move o cursor para a posição desejada (começa em 0).
<br>
### 🔹 _**Informações do terminal**_

- `tput cols` → número de colunas (largura).
- `tput lines` → número de linhas (altura).
- `tput longname` → mostra o nome completo do terminal.
- `tput colors` → mostra quantas cores o terminal suporta.
<br>
### 🔹 **Cores**

- `tput setaf N` → define cor do texto (foreground).
- `tput setab N` → define cor de fundo (background).

```ini

0 = Preto
1 = Vermelho
2 = Verde
3 = Amarelo
4 = Azul
5 = Magenta
6 = Ciano
7 = Branco

```
<br>
### 🔹 **Estilos de texto**

- `tput bold` → negrito.
- `tput dim` → texto "apagado" (menos intenso).
- `tput smul` → sublinhado.
- `tput rmul` → remove sublinhado.
- `tput rev` → inversão (texto ↔ fundo).
- `tput smso` → modo destaque (standout).
- `tput rmso` → desativa destaque.
- `tput sgr0` → reset tudo (volta ao normal).
<br>

### ***Tabela de referência rápida do `tput`***

| **Categoria**               | **Comando `tput`**                           | **Descrição / Uso**                           |
| --------------------------- | -------------------------------------------- | --------------------------------------------- |
| **Controle do terminal**    | `clear`                                      | Limpa a tela                                  |
|                             | `reset`                                      | Reseta o terminal                             |
|                             | `civis`                                      | Esconde o cursor                              |
|                             | `cnorm`                                      | Mostra o cursor                               |
|                             | `cup <linha> <coluna>`                       | Move o cursor (0,0 = canto superior esquerdo) |
| **Informações do terminal** | `cols`                                       | Retorna número de colunas                     |
|                             | `lines`                                      | Retorna número de linhas                      |
|                             | `longname`                                   | Nome completo do terminal                     |
|                             | `colors`                                     | Número de cores suportadas                    |
| **Cores (texto / fundo)**   | `setaf N`                                    | Cor do texto (foreground)                     |
|                             | `setab N`                                    | Cor de fundo (background)                     |
| **Códigos de cores comuns** | 0 = Preto                                    | 1 = Vermelho, 2 = Verde, 3 = Amarelo          |
|                             | 4 = Azul, 5 = Magenta, 6 = Ciano, 7 = Branco |                                               |
| **Estilos de texto**        | `bold`                                       | Negrito                                       |
|                             | `dim`                                        | Texto menos intenso                           |
|                             | `smul`                                       | Sublinhado                                    |
|                             | `rmul`                                       | Remove sublinhado                             |
|                             | `rev`                                        | Inversão texto ↔ fundo                        |
|                             | `smso`                                       | Destaque (standout)                           |
|                             | `rmso`                                       | Remove destaque                               |
|                             | `sgr0`                                       | Reseta formatação                             |
|                             | `sgr0`                                       | Reseta formatação                             |
<br>

## ***Mini-script prático de usos reais***

```sh
#!/bin/bash

# === Configurações de cores ===
verde=$(tput setaf 2)
amarelo=$(tput setaf 3)
vermelho=$(tput setaf 1)
azul=$(tput setaf 4)
reset=$(tput sgr0)

# === Função: barra de progresso ===
progresso() {
    cols=$(tput cols)
    msg="Carregando..."
    echo -n "$msg "
    for i in $(seq 1 $((cols - ${#msg} - 1))); do
        echo -n "#"
        sleep 0.01
    done
    echo
}

# === Início ===
tput clear
cols=$(tput cols)
titulo="SISTEMA DE BACKUP"
pos=$(( (cols - ${#titulo}) / 2 ))

tput cup 1 $pos
echo "${azul}$(tput bold)$titulo${reset}"

# === Menu ===
tput cup 3 5; echo "${verde}1) Iniciar backup${reset}"
tput cup 4 5; echo "${amarelo}2) Ver status${reset}"
tput cup 5 5; echo "${vermelho}3) Sair${reset}"

# === Leitura da escolha ===
tput cup 7 5
read -p "Escolha uma opção: " opcao

case $opcao in
    1)
        echo "Iniciando backup..."
        progresso
        echo "${verde}[OK] Backup concluído!${reset}"
        ;;
    2)
        echo "Verificando status..."
        progresso
        echo "${amarelo}[INFO] Último backup feito há 2h${reset}"
        ;;
    3)
        echo "${vermelho}Saindo...${reset}"
        ;;
    *)
        echo "${vermelho}Opção inválida!${reset}"
        ;;
esac

```
<br>
### ***Demonstração***

```sh

#!/bin/bash

# Limpar a tela no início
tput clear

# === INFO DO TERMINAL ===
echo "$(tput bold)=== Informações do Terminal ===$(tput sgr0)"
echo "Linhas: $(tput lines)"
echo "Colunas: $(tput cols)"
echo "Cores suportadas: $(tput colors)"
echo "Nome do terminal: $(tput longname)"
echo

# === ESTILOS DE TEXTO ===
echo "$(tput bold)=== Estilos de Texto ===$(tput sgr0)"
echo "$(tput bold)Negrito$(tput sgr0)"
echo "$(tput dim)Texto apagado (dim)$(tput sgr0)"
echo "$(tput smul)Sublinhado$(tput rmul)"
echo "$(tput rev)Inversão de cores (rev)$(tput sgr0)"
echo "$(tput smso)Destaque (standout)$(tput rmso)"
echo "$(tput sgr0)Normal (reset)"
echo

# === CORES ===
echo "$(tput bold)=== Cores de Texto (setaf) ===$(tput sgr0)"
for i in {0..7}; do
    echo "$(tput setaf $i)Cor $i$(tput sgr0)"
done
echo

echo "$(tput bold)=== Cores de Fundo (setab) ===$(tput sgr0)"
for i in {0..7}; do
    echo "$(tput setab $i) Fundo $i $(tput sgr0)"
done
echo

# === CURSOR ===
echo "$(tput bold)=== Cursor ===$(tput sgr0)"
echo "Normal (cnorm)"
tput civis
echo "Cursor escondido (civis) → espere 2s"
sleep 2
tput cnorm
echo "Cursor de volta (cnorm)"
echo

# === POSICIONAMENTO ===
echo "$(tput bold)=== Posicionamento ===$(tput sgr0)"
tput cup 20 10
echo "Este texto foi impresso na linha 20, coluna 10"
tput cup 25 0
echo

```
<br>
