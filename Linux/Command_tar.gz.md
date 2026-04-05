 # ***`tar.gz`

O **`.tar.gz`** é um formato muito comum no Linux/Unix para empacotar e comprimir arquivos, ele é composto por duas etapas:

- **`.tar`** → "Tape Archive", junta vários arquivos em **um único arquivo** (sem compressão).
- **`.gz`** → gzip, que **comprime** esse arquivo `.tar`

ou seja

```css
arquivos → .tar (empacotado) → .tar.gz (empacotado + comprimido)
```


## ***Command basic .tar.gz***

- ***Criar um arquivo `.tar.gz`*** -> <b><small>(empacotando e comprimindo arquivo ou pastas)</small></b>
`tar -cvzf myFile.tar.gz arquivos/pastas`

> Esse comando irá criar um arquivo chamado`myFile.tar.gz` com todos os arquivos e pastas que estão no caminho fornecido ao comando, empacotados e comprimidos.

**Options:**
- `c` → create (criar)
- `v` → verbose (opcional, só para ver o processo)
- `z` → compactar com gzip
- `f` → nome do arquivo

> <span style="color:red"><b>OBS:</b> A ordem das opções incluenciam na compactação (seguir ordem correta)</span>

---

 - ***Extraindo arquivos/pastas***
`tar -xvzf myFile.tar.gz`

> Isso cria uma pasta com os arquivos que estão dentro do arquivo  `myFile.tar.gz`.

**Options:**
- `x` → extract (extrair)
- `v` → verbose (mostra os arquivos durante a extração)
- `z` → gzip (indica que está comprimido com gzip
- `f` → file (indica o nome do arquivo)


- ***Extraindo arquivos/pastas para um local especifico.***
`tar -xvzf myFile.tar.gz -C /path_destino/..`

- `C` → path (opção necessária para indicar destino do arquivo descompactado)
 ---

- ***Listar conteúdo de um arquivo .tar.gs sem extrai-lo***
`tar -tvf myFile.tar.gz`

> Esse comando exibe todos os arquivos que estão dentro do `myFile.tar.gz`


---

- ***Compactar varios arquivos.***
`tar -cvzf imagens.tar.gz foto1.jpg foto2.jpg foto3.png`

> Utilizando o command dessa forma, podemos selecionar cada arquivo maualmente para compactação.


---

- ***Criar/Adicionar arquivos a um `.tar` existente*** -> (Só funciona com `.tar`, não `.tar.gz`)
`tar -rvf myFile.tar novo_arquivo.txt`

> Esse comando adicionar arquivos a uma arquivo.tar antes de comprimir


- ***Comprimir um arquivo `.tar`***
`gzip myFile.tar`

 > Esse comando comprime o arquivo `myFile.tar` transformando-o em `myFile.tar.gz`
 

---

## ***Trabalhando com outras comprossões***

- ***.tar.bz2 (bzip2):***

```sh
tar -cvjf arquivo.tar.bz2 pasta/   # criar
tar -xvjf arquivo.tar.bz2          # extrair
```

- ***.tar.xz (xz):***

```sh
tar -cvJf arquivo.tar.xz pasta/   # criar
tar -xvJf arquivo.tar.xz          # extrair
```


### ***Dicas:***

- Se o arquivo estiver **.tar.bz2** (com bzip2), troque <b>`z`</b> por <b>`j`</b>.
- Sempre use <b>`v`</b> se quiser ver o que está acontecendo.
- Para arquivos grandes, <b>`z` (gzip)</b> é mais rápido; <b>`j` (bzip2)</b> comprime mais, </br>mas é mais lento e **`J` (xz)**
- <b>`f`</b> é obrigatório para indicar o arquivo alvo.


## ***Diferença entre os compressores***

| **Formato** | **Extensão** | **Comando no tar** | **Velocidade**                         | **Taxa de compressão**       | **Uso comum**                                                      |
| ----------- | ------------ | ------------------ | -------------------------------------- | ---------------------------- | ------------------------------------------------------------------ |
| **gzip**    | `.gz`        | `-z`               | Muito rápido                           | Média                        | Padrão no Linux, ideal para backups rápidos.                       |
| **bzip2**   | `.bz2`       | `-j`               | Mais lento                             | Melhor que gzip              | Quando precisa de arquivos menores sem se importar com velocidade. |
| **xz**      | `.xz`        | `-J`               | Mais lento que bzip2 (mas pode variar) | Muito alta (melhor dos três) | Distribuições Linux modernas, pacotes oficiais.                    |

## ***Referência rápida!***
**Mapa de resumo completo de comandos `.tar.gz`**

| **Ação**                                    | **Comando**                                  | **Descrição**                                            |
| ------------------------------------------- | -------------------------------------------- | -------------------------------------------------------- |
| **Extrair `.tar.gz`**                       | `tar -xvzf arquivo.tar.gz`                   | Extrai o conteúdo do arquivo.                            |
| **Extrair em pasta específica**             | `tar -xvzf arquivo.tar.gz -C /caminho/pasta` | Extrai para a pasta escolhida.                           |
| **Criar `.tar.gz` de uma pasta**            | `tar -cvzf arquivo.tar.gz pasta/`            | Cria um `.tar.gz` com a pasta ou arquivos especificados. |
| **Criar `.tar.gz` de vários arquivos**      | `tar -cvzf arquivo.tar.gz arquivo1 arquivo2` | Junta e comprime vários arquivos em um só.               |
| **Listar conteúdo**                         | `tar -tvf arquivo.tar.gz`                    | Mostra os arquivos dentro do `.tar.gz` sem extrair.      |
| **Testar integridade**                      | `tar -tzf arquivo.tar.gz`                    | Verifica se o `.tar.gz` está ok (não corrompido).        |
| **Extrair `.tar.bz2`**                      | `tar -xvjf arquivo.tar.bz2`                  | Para arquivos compactados com bzip2.                     |
| **Criar `.tar.bz2`**                        | `tar -cvjf arquivo.tar.bz2 pasta/`           | Compacta com bzip2 em vez de gzip.                       |
| **Adicionar arquivo a um `.tar` existente** | `tar -rvf arquivo.tar novo_arquivo`          | Só funciona em `.tar`, não `.tar.gz`.                    |

