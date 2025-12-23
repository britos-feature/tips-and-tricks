# FS - File System
Module FS - File System
O módulo fs (File System) do Node.js permite interagir com o sistema de arquivos, possibilitando operações como leitura, escrita, criação, exclusão e manipulação de arquivos e diretórios. Ele fornece métodos **síncronos** e **assíncronos** para essas operações. 


### **`fs.readdir()`** Modo de leitura (diretórios)
O método `fs.readdir()` do Node.js pertence ao módulo `fs` (File System) e é utilizado para ler o conteúdo de um diretório. Ele retorna uma lista com os nomes dos arquivos e subdiretórios presentes no diretório especificado. Mais detalhes em  **[[fs.readdir()|Complementos]]**

#### **Sintaxes:**

```js
fs.readdir(path, options, callback)
```

#### Detalhamento:

- **`path`** (string ou Buffer ou URL ou integer): O caminho do diretório a ser lido.
- **`options`** (opcional):
    - Tipo: `string` ou objeto.
    - Define como os resultados serão retornados.
    - Pode especificar a codificação da string e se os resultados devem ser objetos `Dirent` em vez de strings.
- **`callback`** (função): A função chamada quando a operação é concluída. Recebe dois argumentos:
    - **`err`**: Um erro caso ocorra algum problema.
    - **`files`**: Um array contendo os nomes dos arquivos e diretórios dentro do diretório especificado.


#### Exemplo básico

```js
const fs = require('fs');

fs.readdir('./meuDiretorio', (err, files) => {
  if (err) {
    console.error('Erro ao ler diretório:', err);
    return;
  }
  console.log('Arquivos no diretório:', files);
});
```

#### Detalhamento:

- O código lê o conteúdo do diretório `./meuDiretorio`.
- Se houver erro, ele será tratado no `if (err)`.
- Caso contrário, `files` conterá um array de strings com os nomes dos arquivos e pastas do diretório.


#### Usando Opções (`fs.readdir` com `options`)
Você pode usar um objeto de opções para modificar o comportamento da função.

####  Especificando Codificação

```js
fs.readdir('./meuDiretorio', { encoding: 'utf8' }, (err, files) => {
  if (err) throw err;
  console.log('Lista de arquivos:', files);
});

```

> Define a codificação das strings retornadas.


#### Retornando `Dirent` para mais informações

```js
fs.readdir('./meuDiretorio', { withFileTypes: true }, (err, files) => {
  if (err) throw err;
  
  files.forEach(file => {
    console.log(`${file.name} - ${file.isDirectory() ? 'Diretório' : 'Arquivo'}`);
  });
});
```

> Quando utilizado com a opção `{ withFileTypes: true }`, ele retorna objetos do tipo **`Dirent`**, que representam cada entrada do diretório e fornecem métodos úteis para determinar o tipo de cada item.

#### **DIRENT**

DIRENT é um objeto que representa uma entrada em um diretório lido por funções como **`fs.readdir`** ou  **`fs.readdirSync`**, quando a opção **`{ withFileTypes: true }`** é especificada.

**DIRENT** fornece informações sobre o tipo de entrada encontrada como: arquivo, diretório, link simbólico, etc, e permite manipulações baseadas nessas informações.

**Principais métodos de `Dirent`:**

- **`.isFile():`** Retorna true se a entrada for um arquivo.
- **`.isDirectory():`** Retorna true se a entrada for um diretório.
- **`.isBlockDevice():`** Retorna true se a entrada for um dispositivo de bloco.
- **`.isCharacterDevice():`** Retorna true se a entrada for um dispositivo de caractere.
- .**`isSymbolicLink():`** Retorna true se a entrada for um link simbólico.
- **`.isFIFO():`** Retorna true se a entrada for um pipe (FIFO).
- .**`isSocket()`**: Retorna true se a entrada for um socket.

#### Exemplos de utilização **`dirent`**

```js
fs.promises.readdir('/home/britos/JsDocs', { withFileTypes: true })
    .then(response => {
        response.forEach(dirent => {
            if (dirent.isFile()) {
                console.log(`${dirent.name} é um arquivo`);
            }

            else if (dirent.isDirectory()) {
                console.log(`${dirent.name} é um diretório`);
            }

            else if (dirent.isSymbolicLink()) {
                console.log(`${dirent.name} é um link simbólico`);
            }

            else {
                console.log(`${dirent.name} é outro tipo`);
            }
        });

    })
    .catch(err => console.log('Deu ruim', err));
```


 > **`Dirent`** é útil quando você deseja obter informações sobre o tipo de cada entrada sem precisar chamar fs.stat ou fs.lstat separadamente para cada item, o que pode ser mais eficiente.


#### Versão Síncrona (`fs.readdirSync()`)
Se precisar bloquear a execução até a leitura terminar, use a versão síncrona:

```js
const files = fs.readdirSync('./meuDiretorio');
console.log('Arquivos no diretório:', files);
```

> - Esta função retorna diretamente o array de nomes de arquivos, sem precisar de callback.
- Pode ser útil em scripts curtos, mas não é recomendada para aplicações de alto desempenho devido ao bloqueio da thread principal.


#### **`Usando `fs.promises.readdir()` (Async/Await)`**
Para código assíncrono mais moderno e legível, utilize a versão baseada em `Promises`:


```js
const fs = require('fs').promises;

async function lerDiretorio() {
  try {
    const files = await fs.readdir('./meuDiretorio');
    console.log('Arquivos:', files);
  } catch (err) {
    console.error('Erro ao ler diretório:', err);
  }
}

lerDiretorio();
```

**Vantagens**:

- Evita o uso de callbacks aninhados (callback hell).
- Permite o uso de `try/catch` para tratamento de erros.
- Melhor integração com `async/await`.

#### Diferença entre `fs.readdir()` e `fs.readdirSync()`

| Método                  | Tipo                 | Bloqueia Execução? | Retorna                |
| ----------------------- | -------------------- | ------------------ | ---------------------- |
| `fs.readdir()`          | Assíncrono           | Não                | Callback (erro, array) |
| `fs.readdirSync()`      | Síncrono             | Sim                | Array de strings       |
| `fs.promises.readdir()` | Assíncrono (Promise) | Não                | Promise<Array>         |

#### Quando usar qual?

- **`fs.readdir()`**: Para aplicações que precisam ser responsivas e não devem bloquear a execução.
- **`fs.readdirSync()`**: Para scripts rápidos ou inicialização onde o bloqueio não é um problema.
- **`fs.promises.readdir()`**: Quando se quer um código assíncrono mais moderno, evitando callbacks.


#### **Tratamento de Erros Comuns**

```js

// diretório inexistene
Error: ENOENT: no such file or directory, scandir 'caminho/invalido'


// Verifique se o diretório existe antes de tentar ler.
Error: EACCES: permission denied, scandir 'diretorio/protegido'
// Error: EACCES: permission denied, scandir 'diretorio/protegido'


// **Número excessivo de arquivos**:
// Se houver muitos arquivos, a execução pode ser lenta. Use `fs.readdir()` em conjunto com streaming se necessário.
```


