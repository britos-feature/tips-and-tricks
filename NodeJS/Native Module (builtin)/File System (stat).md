O **`fs.stat()`** é um método do módulo `fs` (**File System**) que **consulta o sistema operacional** para obter os **metadados** de um arquivo ou diretório.

Essas informações vêm do **inode** (no Linux e macOS) ou da **tabela de arquivos do sistema (NTFS, FAT, etc.)** no Windows.

**Esses metadados incluem:**

- Tipo do item (arquivo, pasta, link simbólico, etc.)
- Tamanho em bytes
- Permissões
- Dono e grupo (UID/GID)
- Datas de criação, modificação e acesso
<br>
### **Existem três variantes principais**

|Método|Descrição|Quando usar|
|---|---|---|
|**`fs.stat()`**|Obtém informações e **segue links simbólicos** (ou seja, mostra os dados do arquivo apontado pelo link).|Caso comum, quando você quer saber informações reais do arquivo.|
|**`fs.lstat()`**|Obtém informações **sem seguir links simbólicos** (mostra dados do link em si).|Quando você quer saber se algo é um link e suas propriedades.|
|**`fs.fstat()`**|Obtém informações a partir de um **descritor de arquivo (file descriptor)** já aberto.|Quando você já abriu o arquivo com `fs.open()`.
<br> 
### **Modo de Utilização `fs.stat()`

- **Callback (assíncrono tradicional)**

```js

// CallBack

const fs = require('fs');

fs.stat('arquivo.txt', (err, stats) => {
	if (err) {
	    return console.error('Erro:', err.message);
	}

  console.log('É arquivo?', stats.isFile());
  console.log('É diretório?', stats.isDirectory());
  console.log('Tamanho:', stats.size, 'bytes');
  console.log('Última modificação:', stats.mtime);
});

```
<br>
- **Síncrono**

```js

// Síncrono

const fs = require('fs');

try {
	const stats = fs.statSync('arquivo.txt');
	console.log('Tamanho do arquivo:', stats.size);
} 
catch (err) {
	console.error('Erro:', err.message);
}

```
<br>
- **Promises (moderno e ideal)**

```js

// Assícrono (async - await)

const fs = require('fs').promises;

async function mostrarInfo() {
	try {
	    const stats = await fs.stat('arquivo.txt');
	    console.log('Arquivo criado em:', stats.birthtime);
	    console.log('Última modificação:', stats.mtime);
	}
	catch (err) {
	    console.error('Erro:', err.message);
	}
}

mostrarInfo();

```
<br>
> **Recomendado,** Código limpo e compatível com `async/await`.
<br>
---

### **Detalhando o objeto `fs.Stats`**
O retorno de `fs.stat()` é um **objeto da classe `fs.Stats`**.  
Veja suas principais **propriedades e métodos**:

🧠 Métodos de verificação

|Método|Retorna `true` se...|
|---|---|
|`isFile()`|o item for um arquivo|
|`isDirectory()`|for um diretório|
|`isSymbolicLink()`|for um link simbólico (somente em `lstat`)|
|`isSocket()`|for um socket|
|`isBlockDevice()`|for um dispositivo de bloco (ex: HD)|
|`isCharacterDevice()`|for um dispositivo de caractere|
|`isFIFO()`|for um _pipe_ FIFO|
<br>
📅 Propriedades de data/hora

| Propriedade | Descrição                             |
| ----------- | ------------------------------------- |
| `atime`     | Data do último acesso                 |
| `mtime`     | Data da última modificação (conteúdo) |
| `ctime`     | Data da última alteração de metadados |
| `birthtime` | Data de criação do arquivo            |
<br>
⚖️ Outras propriedades úteis

| Propriedade | Significado                                                  |
| ----------- | ------------------------------------------------------------ |
| `size`      | Tamanho em bytes                                             |
| `mode`      | Permissões do arquivo (em octal, ex: `0o755`)                |
| `uid`       | á armazenado                                                 |
| `gid`       | ID do grupo dono                                             |
| `ino`       | Número do inode (identificador único no sistema de arquivos) |
| `dev`       | Identificador do dispositivo onde o arquivo está armazenado  |
<br>

----

### **Exemplo comparando `stat()`, `lstat()` e `fstat()`**

```js

const fs = require('fs').promises;

async function comparar() {
  try {
    const stat = await fs.stat('meuLink.txt');
    const lstat = await fs.lstat('meuLink.txt');

    console.log('--- fs.stat() ---');
    console.log('Segue o link?', stat.isFile());
    console.log('É link simbólico?', stat.isSymbolicLink()); // false

    console.log('--- fs.lstat() ---');
    console.log('Segue o link?', lstat.isFile()); // false
    console.log('É link simbólico?', lstat.isSymbolicLink()); // true
  } catch (err) {
    console.error('Erro:', err);
  }
}

comparar();

```
<br>
**Resumo:**

| Método               | Bloqueia? | Segue link? | Base                 |
| -------------------- | --------- | ----------- | -------------------- |
| `fs.stat()`          | ❌ Não     | ✅ Sim       | Caminho              |
| `fs.lstat()`         | ❌ Não     | ❌ Não       | Caminho              |
| `fs.fstat()`         | ❌ Não     | ✅ Sim       | Descritor de arquivo |
| `fs.statSync()`      | ✅ Sim     | ✅ Sim       | Caminho              |
| `fs.promises.stat()` | ❌ Não     | ✅ Sim       | Caminho              |

---

### **Exemplo prático e completo usando `fs.stat()`**
**Listar o conteúdo de um diretório** e exibir informações detalhadas de cada item.

#### 🧠 Objetivo

O script vai:

1. Ler um diretório (ex: `./meus_arquivos`);
2. Para cada item, usar `fs.stat()` para obter detalhes;
3. Mostrar tipo (arquivo/pasta), tamanho e datas.
<br>
- Exemplo completo usando **`async/await`** e **`fs.promises`**

```js

const fs = require('fs').promises;
const path = require('path');

async function listarDiretorio(diretorio) {
  try {
    // Lê o conteúdo do diretório
    const arquivos = await fs.readdir(diretorio);

    console.log(`📂 Conteúdo de: ${diretorio}\n`);

    // Percorre cada item dentro da pasta
    for (const nome of arquivos) {
      const caminhoCompleto = path.join(diretorio, nome);
      const stats = await fs.stat(caminhoCompleto);

      // Verifica se é arquivo ou diretório
      const tipo = stats.isDirectory() ? '📁 Diretório' : '📄 Arquivo';

      console.log(`${tipo}: ${nome}`);
      console.log(`  ├─ Tamanho: ${stats.size} bytes`);
      console.log(`  ├─ Criado em: ${stats.birthtime}`);
      console.log(`  └─ Última modificação: ${stats.mtime}\n`);
    }

  } catch (err) {
    console.error('Erro ao listar diretório:', err.message);
  }
}

// Chame a função passando o caminho da pasta
listarDiretorio('./meus_arquivos');

```
<br>
### **Explicando passo a passo**

**`fs.readdir()`**  
    → lê os nomes dos arquivos/pastas dentro do diretório.

**`path.join()`**
	→ cria o caminho completo, independente do sistema operacional.

**`fs.stat()`**  
    → obtém informações detalhadas de cada item.

**`stats.isDirectory()` / `stats.isFile()`**
	→ diferencia arquivos de pastas.

**Template de log**
	→ imprime os resultados de forma organizada.
<br>
### **Exemplo de saída**

```js

📂 Conteúdo de: ./meus_arquivos

📁 Diretório: imagens
  ├─ Tamanho: 4096 bytes
  ├─ Criado em: 2025-10-02T12:15:42.000Z
  └─ Última modificação: 2025-10-03T14:22:10.000Z

📄 Arquivo: texto.txt
  ├─ Tamanho: 1024 bytes
  ├─ Criado em: 2025-09-28T09:41:33.000Z
  └─ Última modificação: 2025-10-01T11:10:05.000Z

```
<br>
### **Variação: usando `fs.lstat()` para identificar _links simbólicos_**

```js

const stats = await fs.lstat(caminhoCompleto);

if (stats.isSymbolicLink()) {
  console.log(`🔗 Link simbólico: ${nome}`);
}

```
<br>
> Isso é útil se você tiver links simbólicos e quiser tratá-los separadamente.
<br>
### **Dica extra**
Quer melhorar o desempenho?  
Use **`Promise.all()`** para fazer o **`stat()`** de todos os arquivos em paralelo:

```js

await Promise.all(
  arquivos.map(async (nome) => {
    const caminhoCompleto = path.join(diretorio, nome);
    const stats = await fs.stat(caminhoCompleto);
    console.log(`${nome} → ${stats.isDirectory() ? 'pasta' : 'arquivo'} (${stats.size} bytes)`);
  })
);

```
<br>

