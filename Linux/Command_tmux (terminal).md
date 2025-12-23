O **tmux** (Terminal MUltipleXer) é uma ferramenta de **multiplexação de terminais** no Linux/Unix.  
Ele permite que você **divida uma única sessão de terminal em várias janelas e painéis**, além de **manter processos rodando em background** mesmo que você feche o terminal.

Em resumo, o tmux serve para:

- Ter **múltiplos terminais dentro de um só**.
- **Manter sessões ativas** (mesmo se desconectar do SSH, por exemplo).
- **Organizar seu trabalho** (dividir tela em janelas/painéis).
- **Compartilhar sessões** entre usuários (útil em pair programming).


## ***Conceitos principais***

1. **Sessão**
    - É como uma “instância” do tmux.
    - Você pode ter várias sessões rodando e alternar entre elas.

2. **Janela**
    - Cada sessão pode ter várias **janelas** (como abas).
    - Cada janela roda um shell/programa independente.

3. **Painel**
    - Dentro de uma janela, você pode dividir a tela em **painéis** (verticais ou horizontais).


## ***Comandos básicos***

- ***Iniciar o tmux***
`tmux` <small><b>or</b></small> `tmux new -s mySession`

- ***


