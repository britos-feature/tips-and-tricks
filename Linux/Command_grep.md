# 📝 Comando `grep`, `egrep`, `fgrep`, `--color` e Expressões Regulares

O comando **`grep`** é utilizado para **buscar padrões de texto** em arquivos ou fluxos de entrada, ele suporta tanto buscas simples quanto consultas avançadas utilizando **expressões regulares**.

Resumindo, o `grep` (**G**lobal **R**egular **E**xpression **P**rint) procura linhas que **correspondem a um padrão** e exibe essas linhas como saída.

---

### ✔️ Sintaxe básica

```bash
grep [opções] padrão arquivo
```

Exemplo simples:

```bash
grep "erro" app.log
```

---

# ⚙️ 2. Funcionamento Interno (Simplificado)

1. O grep lê linha por linha do arquivo ou da entrada.
2. Aplica o padrão informado (string simples ou regex).
3. Se houver correspondência, a linha é impressa.
4. Caso contrário, é ignorada.

O grep utiliza algoritmos eficientes como **Boyer-Moore**, garantindo alta performance.

---

# 🎛️ 3. Principais Opções do `grep`

### **3.1 Ignorar maiúsculas/minúsculas**

```bash
grep -i "palavra" arquivo.txt
```

### **3.2 Exibir números de linha**

```bash
grep -n "falha" log.txt
```

### **3.3 Mostrar apenas arquivos com correspondência**

```bash
grep -l "token" *.txt
```

### **3.4 Mostrar linhas que _não_ combinam**

```bash
grep -v "OK" status.txt
```

### **3.5 Busca recursiva em diretórios**

```bash
grep -r "senha" /etc/
```

### **3.6 Exibir contexto (linhas antes/depois)**

```bash
grep -C 3 "fatal" log.txt   # 3 antes e 3 depois
grep -A 2 "fatal" log.txt   # apenas depois
grep -B 2 "fatal" log.txt   # apenas antes
```

---

# 🌈 4. Usando `--color` para destacar correspondências

O grep pode realçar visualmente as correspondências com cores.

### ✔️ Exemplo:

```bash
grep --color=auto "root" /etc/passwd
```

O padrão é destacado em **vermelho**, facilitando a leitura.

Também é possível usar:

```bash
export GREP_OPTIONS="--color=auto"
```

_(obsoleto em versões recentes)_

Ou via variável:

```bash
alias grep='grep --color=auto'
```

---

# 🔎 5. Diferenças entre `grep`, `egrep`, `fgrep`

O comportamento dos três comandos é semelhante, mas com diferenças importantes.

| Comando   | Descrição                                                                   |
| --------- | --------------------------------------------------------------------------- |
| **grep**  | Usa expressões regulares "básicas" (BRE).                                   |
| **egrep** | Usa regex estendidas (ERE). Equivalente a `grep -E`.                        |
| **fgrep** | Busca **strings literais**, sem interpretar regex. Equivalente a `grep -F`. |

---

## ✔️ 5.1 `grep` (BRE – Basic Regular Expressions)

Suporta regex básicas. Para usar quantificadores como `+` e `?`, é preciso escapar:

```bash
grep "erro\+" arquivo.txt
```

---

## ✔️ 5.2 `egrep` ou `grep -E` (ERE – Extended Regular Expressions)

Regex estendidas, mais poderosas e sem necessidade de escape.

Exemplo:

```bash
egrep "erro|falha|critico" log.txt
```

Permite:

- Alternância: `a|b`
- Grupo: `( )`
- Quantificadores: `+`, `?`, `{n}` sem escapes

---

## ✔️ 5.3 `fgrep` ou `grep -F` (String literal)

Não interpreta regex. Mais rápido para buscas literais.

Exemplo:

```bash
fgrep "*.*" arquivo.txt   # Procura literalmente *.*
```

Ideal para buscar padrões com caracteres especiais sem querer regex.

---

# 🧠 6. Expressões Regulares no `grep`

## ✔️ 6.1 Meta-caracteres principais (BRE e ERE)

| Meta  | Significado           | Exemplo              |
| ----- | --------------------- | -------------------- |
| `.`   | Qualquer caractere    | `a.b` encontra `a*b` |
| `^`   | Início da linha       | `^erro`              |
| `$`   | Final da linha        | `fim$`               |
| `[]`  | Classe de caracteres  | `[0-9]`              |
| `[^]` | Negação               | `[^0-9]`             |
| `*`   | Repetição (0 ou mais) | `ab*`                |

---

## ✔️ 6.2 Quantificadores (apenas ERE sem escape)

| Quantificador | Significado  |
| ------------- | ------------ |
| `+`           | 1 ou mais    |
| `?`           | 0 ou 1       |
| `{n}`         | exatamente n |
| `{n,}`        | n ou mais    |
| `{n,m}`       | entre n e m  |

Exemplo:

```bash
grep -E "erro{2,}" arquivo.log
```

---

## ✔️ 6.3 Alternância (ERE)

```bash
grep -E "erro|falha|critico" log.txt
```

---

## ✔️ 6.4 Grupos

```bash
grep -E "(GET|POST) /api" access.log
```

---

# 🧰 7. Exemplos Práticos

### Encontrar IPs

```bash
grep -E "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" log.txt
```

### Contar ocorrências

```bash
grep -c "timeout" server.log
```

### Buscar arquivo contendo determinada palavra

```bash
grep -rl "API_KEY" .
```

### Filtrar saída de outro comando (pipe)

```bash
ps aux | grep firefox
```

---

# 📝 8. Resumo Completo

- **grep** → regex básicas (BRE)
- **egrep / grep -E** → regex estendidas (ERE), mais práticas
- **fgrep / grep -F** → busca literal, sem regex
- **--color** → destaca correspondências
- Regex permitem buscas simples e avançadas
- Pode ser usado com pipes, contexto, recursivo, etc.

---

---

# 📎 **Cartão de Bolso – Comando `grep` (Cheat Sheet)**

## 🔍 **Básico**

```bash
grep "padrão" arquivo
grep -i "padrão" arquivo     # Ignora maiúsc./minúsc.
grep -n "padrão" arquivo     # Mostra número da linha
grep -v "padrão" arquivo     # Linhas que NÃO combinam
grep -r "padrão" diretório   # Busca recursiva
```

---

## 🌈 **Destaque com cores**

```bash
grep --color=auto "padrão" arquivo
alias grep='grep --color=auto'
```

---

## 🔎 **Contexto**

```bash
grep -A 2 "erro" log.txt   # Depois
grep -B 2 "erro" log.txt   # Antes
grep -C 2 "erro" log.txt   # Antes + Depois
```

---

# 🧠 **grep x egrep x fgrep**

### **grep** – BRE (Basic Regex)

- Interpreta regex básica
- Quantificadores precisam de `\`

### **egrep / grep -E** – ERE (Extended Regex)

- Regex estendida
- Suporta: `()`, `|`, `+`, `?`, `{n}` SEM escape

```bash
grep -E "erro|falha|critico" log.txt
```

### **fgrep / grep -F**

- Busca literal, **não** interpreta regex
- Mais rápido

```bash
grep -F "*.*" arquivo.txt
```

---

# 🧩 **Regex Essencial**

## ✔️ Metacaracteres

| Símbolo | Significado          |
| ------- | -------------------- |
| `.`     | Qualquer caractere   |
| `^`     | Início da linha      |
| `$`     | Fim da linha         |
| `[]`    | Classe de caracteres |
| `[^]`   | Negação              |
| `*`     | 0 ou mais repetições |

---

## ✔️ ERE (usado com `grep -E` ou `egrep`)

| Símbolo | Significado  |     |
| ------- | ------------ | --- |
| `+`     | 1 ou mais    |     |
| `?`     | 0 ou 1       |     |
| `       | `            | OU  |
| `{n}`   | exatamente n |     |
| `{n,}`  | n ou mais    |     |
| `{n,m}` | entre n e m  |     |
| `()`    | Agrupamento  |     |

---

# 🚀 **Exemplos Rápidos**

### Encontrar IPs:

```bash
grep -E "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" log.txt
```

### Buscar múltiplas palavras:

```bash
grep -E "erro|falha|critico" log.txt
```

### Linhas que NÃO contêm algo:

```bash
grep -v "OK" status.txt
```

### Mostrar apenas o nome dos arquivos:

```bash
grep -rl "token" .
```

### Contar ocorrências:

```bash
grep -c "timeout" server.log
```

---

# 🛠️ **Uso com Pipes**

```bash
ps aux | grep nginx
dmesg | grep -i usb
netstat -tunlp | grep 443
```
