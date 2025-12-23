# FS - File System
Module FS - File System
O módulo fs (File System) do Node.js permite interagir com o sistema de arquivos, possibilitando operações como leitura, escrita, criação, exclusão e manipulação de arquivos e diretórios. Ele fornece métodos **síncronos** e **assíncronos** para essas operações. 


### **`fs.stat()`** informações sobre (arquivos/diretórios)
O método `fs.stat()` do Node.js faz parte do módulo `fs` (File System) e é usado para obter informações sobre um arquivo ou diretório. Ele retorna um objeto do tipo `fs.Stats`, que contém vários detalhes, como tamanho do arquivo, permissões, data de modificação, entre outros. Mais detalhes em  **[[fs.stat()|Complementos]]**
#### Sintaxe:

```js
const fs = require('fs');

fs.stat('caminho/do/arquivo_ou_diretorio', (err, stats) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(stats);
});
```

#### **Parâmetros**

- `path` _(string | Buffer | URL | integer)_ → Caminho do arquivo ou diretório.
- `callback` _(function)_ → Função chamada com dois argumentos:
    - `err` → Objeto de erro (caso ocorra).
    - `stats` → Objeto `fs.Stats` contendo os detalhes do arquivo/diretório.



#### **Exemplo de uso `fs.stat`**
**Modo Assíncrono (callback)**

```js
const fs = require('fs');

fs.stat('arquivo.txt', (err, stats) => {
  if (err) {
    console.error('Erro ao obter informações:', err);
    return;
  }
  console.log('Detalhes do arquivo:', stats);
});
```


**Modo Síncrono**

```js
const fs = require('fs');

try {
  const stats = fs.statSync('arquivo.txt');
  console.log('Detalhes do arquivo:', stats);
} catch (err) {
  console.error('Erro ao obter informações:', err);
}
```


**Modo com Promises**

```js
const fs = require('fs/promises');

async function obterInfo() {
  try {
    const stats = await fs.stat('arquivo.txt');
    console.log('Detalhes do arquivo:', stats);
  } catch (err) {
    console.error('Erro:', err);
  }
}

obterInfo();
```


#### **Objeto `fs.Stats` e suas Propriedades**
O método `fs.stat()` retorna um objeto `fs.Stats`, que contém informações sobre o arquivo ou diretório.

#### **Principais Propriedades (`fs.stat`)**

| Propriedade              | Tipo      | Descrição                                        |
| ------------------------ | --------- | ------------------------------------------------ |
| `stats.isFile()`         | `boolean` | Retorna `true` se for um arquivo.                |
| `stats.isDirectory()`    | `boolean` | Retorna `true` se for um diretório.              |
| `stats.isSymbolicLink()` | `boolean` | Retorna `true` se for um link simbólico.         |
| `stats.size`             | `number`  | Tamanho do arquivo em bytes.                     |
| `stats.birthtime`        | `Date`    | Data de criação do arquivo.                      |
| `stats.mtime`            | `Date`    | Data da última modificação do arquivo.           |
| `stats.ctime`            | `Date`    | Data da última mudança nos metadados do arquivo. |
| `stats.atime`            | `Date`    | Data do último acesso ao arquivo.                |
| `stats.mode`             | `number`  | Permissões do arquivo no formato numérico.       |
| `stats.uid`              | `number`  | ID do usuário dono do arquivo.                   |
| `stats.gid`              | `number`  | ID do grupo dono do arquivo.                     |

#### **Exemplo de uso `fs.stat() com as propriedades**

```js
const fs = require('fs');

fs.stat('arquivo.txt', (err, stats) => {
  if (err) {
    console.error('Erro:', err);
    return;
  }

  console.log(`É um diretório? ${stats.isDirectory()}`);
  console.log(`É um arquivo? ${stats.isFile()}`);
  console.log(`Tamanho: ${stats.size} bytes`);
  console.log(`Criado em: ${stats.birthtime}`);
  console.log(`Última modificação: ${stats.mtime}`);
});
```


