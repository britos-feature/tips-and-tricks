# **_Command `declare`_**

O comando **`declare`** é usado para **criar, configurar e atribuir propriedades a variáveis** e funções. Ele é como uma “versão turbinada” de `VAR=valor`, pois permite especificar **tipos** e **atributos** para as variáveis.

O **`declare`** não é um comando externo (como `/bin/ls`), e sim um **comando interno do próprio Bash (builtin)**.

Isso significa que ele não roda um programa separado, e sim interage diretamente com a **tabela interna de variáveis** que o Bash mantém na memória.

#### **_Funções principais_**

1. **Criar variáveis com atributos específicos**
2. **Definir variáveis somente-leitura**
3. **Definir variáveis numéricas**
4. **Criar arrays**
5. **Listar variáveis e atributos**

#### **\*Opções mais usadas\*\***

| Opção | Função                                                                         |
| ----- | ------------------------------------------------------------------------------ |
| `-a`  | Declara um **array indexado**                                                  |
| `-A`  | Declara um **array associativo**                                               |
| `-i`  | Trata a variável como **inteiro** (conversão automática de string para número) |
| `-r`  | Torna a variável **somente leitura** (como `readonly`)                         |
| `-x`  | Exporta a variável para o ambiente (como `export`)                             |
| `-p`  | Lista variáveis com atributos (útil para ver configurações)                    |
| `-f`  | Lista funções                                                                  |
| `-F`  | Lista apenas nomes de funções (sem conteúdo)                                   |

**Confirmando que o comando `declare` é um builtin**

```bash
type declare
# Saída: declare is a shell builtin
```

## **_Onde o Bash guarda as variáveis_**

Dentro do Bash, existe uma **tabela hash** interna que armazena todas as variáveis.  
Cada entrada dessa tabela tem:

- **Nome** → ex.: `"PATH"`, `"HOME"`, `"myVar"`.
- **Valor** → o conteúdo da variável.
- **Atributos** → flags internas, como:
  - `readonly` → não pode ser alterada.
  - `integer` → só aceita valores numéricos.
  - `export` → vai para o ambiente (`env`) e é herdada por subprocessos.
  - `array` → é um array indexado.
  - `assoc` → é um array associativo.

Esses atributos são definidos e alterados pelo **`declare`**.

---

## **_O que acontece quando você roda_**

```bash
declare -i number=10
```

1. Cria (ou localiza) uma entrada `"number"` na tabela interna
2. Marca o atributo `"integer"` na estrutura dessa variável
3. Converte `10` para número e armazena internamente.
4. Na hora de atribuir novos valores, ele **automaticamente converte strings para número** (ou 0 se não for possível)

## **_Quando você usa `declare -x`_**

```bash
declare -x myVar=123
```

1. Marca a variável com o atributo `"export"`.
2. Adiciona (ou atualiza) essa variável na **lista de variáveis de ambiente**.
3. Quando você cria um subprocesso (ex.: rodando `ls`), essa variável é copiada para o **env** do processo filho.

Você pode ver essas variáveis com:

```bash
env | grep myVar
```

---

## **_Diferença `declare` vs `export` vs `readonly`_**

- **`export VAR=valor`** → apenas marca a variável para ser herdada
- **`readonly VAR=valor`** → apenas impede modificação
- **`declare`** → pode fazer as duas coisas **e mais**, num só comando.

## **_Listagem e inspeção interna_**

O `declare -p` mostra não só o valor, mas também **o tipo/atributos** que o Bash armazenou:

```bash
declare -i numero=42
declare -p numero
# Saída:
# declare -i numero="42"
```

---

## **_Em termos de código-fonte_**

Se você abrir o código do Bash (GNU Bash, arquivo `variables.c`)

- **`set_var()`** → função que cria ou altera variáveis.
- **`set_var_attribute()`** → função que adiciona atributos (readonly, export, integer).
- Estrutura interna usada:
  ```c
  typedef struct {
      char *name;    // nome da variável
      char *value;   // valor
      int attributes; // flags como export, readonly, integer...
  } SHELL_VAR;
  ```

O `declare` basicamente é só uma **interface para mexer nessas flags** e atribuir valor.

**Resumo:**

- `declare` não cria “um tipo diferente de variável” — ele apenas adiciona **metadados/atributos** na estrutura da variável.
- Por ser builtin, ele atua diretamente na memória do Bash, sem precisar chamar processos externos.
- Ele é mais poderoso que `export` e `readonly` porque une várias funções em um só comando.
