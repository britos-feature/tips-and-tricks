O comando **`top`** no Linux é uma ferramenta de monitoramento em tempo real dos processos que estão rodando no sistema.  
Ele mostra o **uso de CPU, memória, tempo de execução e outros detalhes** sobre os processos ativos.

É como o "Gerenciador de Tarefas" do Windows, só que no terminal.

Quando você abre o `top`, ele divide a tela em **duas seções principais**: cabeçalho (resumo do sistema) e lista de processos.

### **Cabeçalho (resumo do sistema)**

```shell
top - 15:32:01 up 2:10,  1 user,  load average: 0.25, 0.33, 0.40
Tasks: 120 total,   1 running, 119 sleeping,   0 stopped,   0 zombie
%Cpu(s):  3.2 us,  1.1 sy,  0.0 ni, 95.0 id,  0.5 wa,  0.0 hi,  0.2 si
MiB Mem :  7890.3 total,  3102.4 free,  2011.3 used,  2776.6 buff/cache
MiB Swap:  2048.0 total,  2048.0 free,     0.0 used.  5000.0 avail Mem
```

**significado:**

- **_Linha 1_** – informações gerais: - Hora atual (`15:32:01`) - Tempo ligado (`up 2:10`) - Usuários logados (`1 user`) - Carga do sistema (load average) – média de processos na fila de CPU nos últimos **1, 5 e 15 minutos**
- **_Linha 2_** – tarefas: - Total de processos - Quantos estão **running** (ativos), **sleeping** (dormindo), **stopped** (parados) e **zombie** (zumbis – processos que terminaram mas ainda têm entrada na tabela de processos)
- **_Linha 3_** – CPU: - `%us` → tempo de CPU usado por processos do usuário - `%sy` → tempo de CPU usado pelo sistema/kernel - `%ni` → processos com **nice** diferente de 0 (prioridade alterada) - `%id` → tempo ocioso - `%wa` → espera de I/O (disco, rede) - `%hi` → interrupções de hardware - `%si` → interrupções de software
- **_Linha 4_** – Memória RAM: - `total`, `free`, `used` e `buff/cache` - **buff/cache** → memória usada pelo sistema para cache de arquivos (pode ser liberada se necessário)
- **\*Linha 5** – Swap:
  - `total`, `used`, `free` e **avail Mem** (RAM disponível incluindo buffers)

### **_Lista de processos_**

```shell
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1234 root      20   0  162000  12000   8000 S  10.0  0.5   0:02.34 apache2
```

**significados das colunas**

| Coluna  | Significado                                                             |
| ------- | ----------------------------------------------------------------------- |
| PID     | Identificador único do processo                                         |
| USER    | Usuário dono do processo                                                |
| PR      | Prioridade do processo                                                  |
| NI      | Valor nice (ajuste de prioridade, -20 mais prioridade, +19 menos)       |
| VIRT    | Memória virtual usada (RAM + swap + mapeamento)                         |
| RES     | Memória residente (RAM real consumida)                                  |
| SHR     | Memória compartilhada (bibliotecas etc.)                                |
| S       | Estado do processo (`R`=running, `S`=sleeping, `Z`=zombie, `T`=stopped) |
| %CPU    | Percentual de uso da CPU                                                |
| %MEM    | Percentual de uso da RAM                                                |
| TIME+   | Tempo total de CPU gasto pelo processo                                  |
| COMMAND | Nome do comando ou programa                                             |

### **_## Comandos internos do `top`_**

Enquanto o `top` está rodando, você pode usar várias teclas para interagir

| Tecla | Função                                   |
| ----- | ---------------------------------------- |
| **q** | Sair do `top`                            |
| **h** | Mostrar ajuda                            |
| **k** | Matar processo (pede PID)                |
| **r** | Alterar prioridade do processo (renice)  |
| **P** | Ordenar por uso de CPU                   |
| **M** | Ordenar por uso de memória               |
| **T** | Ordenar por tempo total de CPU           |
| **1** | Mostrar cada núcleo de CPU separadamente |
| **f** | Escolher quais colunas mostrar           |
| **u** | Filtrar por usuário específico           |

**exemplos de usos**

**Ordenar por CPU**
`top -o %CPU`

**Ordenar por memória**
`top -o %MEM`

**Processos por usuários**
`top -u name_user`

**Atualizar a cada 2 segundos**
`top -d 2`

### **_Guia Rápido de Filtros e Comandos do `top`_**

| Tecla | Função                                     |
| ----- | ------------------------------------------ |
| **q** | Sair                                       |
| **h** | Ajuda                                      |
| **k** | Matar processo (pede PID)                  |
| **r** | Alterar prioridade (_renice_)              |
| **P** | Ordenar por CPU                            |
| **M** | Ordenar por Memória                        |
| **T** | Ordenar por tempo de CPU                   |
| **1** | Mostrar cada núcleo da CPU                 |
| **o** | Adicionar filtro                           |
| **=** | Limpar filtros                             |
| **W** | Salvar configurações/filtros em `~/.toprc` |

### **_Campos para filtrar (`o` dentro do `top`)_**

| Filtro          | Exemplo         | Resultado                            |
| --------------- | --------------- | ------------------------------------ |
| Usuário         | `USER=root`     | Só processos do root                 |
| Excluir usuário | `USER!=alex`    | Todos, exceto alex                   |
| PID             | `PID=1234`      | Só o processo 1234                   |
| PID maior que   | `PID>1000`      | Processos com PID alto               |
| CPU             | `%CPU>10`       | Processos usando mais de 10% de CPU  |
| CPU menor       | `%CPU<50`       | Processos abaixo de 50% CPU          |
| Memória         | `%MEM>5`        | Usando mais de 5% da RAM             |
| Comando         | `COMMAND=nginx` | Só processos "nginx"                 |
| Excluir comando | `COMMAND!=bash` | Todos, exceto bash                   |
| Prioridade      | `PR<20`         | Prioridade abaixo de 20              |
| Nice            | `NI=-5`         | Processos com nice -5                |
| Estado          | `S=R`           | Só processos em execução (_running_) |

### Estados de processo (coluna `S`)

- **R** → Running (em execução)
- **S** → Sleeping (ocioso/dormindo)
- **Z** → Zombie (finalizado, mas ainda listado)
- **T** → Stopped (parado/suspenso)

### Exemplos de combinação de filtros

Dentro do `top`, pressione `o` várias vezes para empilhar filtros:

- Processos do **root** com mais de **10% CPU**:

```shell
USER=root
%CPU>10
```

- Processos **não root**, consumindo **mais de 5% de RAM**, em execução

```shell
USER!=root
%MEM>5
S=R
```

- Somente processos do **nginx**

```shell
COMMAND=nginx
```

### Dica para scripts (modo batch)

Rodar `top` só uma vez, útil para monitorar via script:

```shell
top -b -n 1 | grep nginx
```

> (`-b` = batch mode, `-n 1` = apenas 1 atualização)

**Resumo final**:

- **`o`** → filtra
- **`P/M/T`** → ordena
- **`k`** → mata processo
- **`r`** → muda prioridade
- **`1`** → vê uso de cada CPU
- **`W`** → salva seu layout e filtros
