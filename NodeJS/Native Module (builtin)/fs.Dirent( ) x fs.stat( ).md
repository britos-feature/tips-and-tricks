Embora **`fs.stat()`** e **`fs.Dirent()`** pareçam fazer coisas parecidas (ambos fornecem informações sobre arquivos e diretórios), **eles são bem diferentes em finalidade, origem e profundidade de dados**.

---

## `fs.stat()
Consulta detalhada no sistema de arquivos

### O que é

- Um **método** que retorna um **objeto `fs.Stats`**.
- Esse objeto contém **metadados completos** sobre o item (arquivo, diretório, link etc.).
- Ele faz uma **chamada ao sistema operacional**, o que significa **I/O de disco** (mais lento, mas mais detalhado).

### Exemplo

```js

const fs = require('fs');

fs.stat('arquivo.txt', (err, stats) => {
  if (err) throw err;
  console.log('É arquivo?', stats.isFile());
  console.log('Tamanho:', stats.size);
  console.log('Criado em:', stats.birthtime);
});

```
<br>
### **O que retorna (`fs.Stats`)**

| Tipo         | Exemplo                                         |
| ------------ | ----------------------------------------------- |
| Tipo de item | `isFile()`, `isDirectory()`, `isSymbolicLink()` |
| Tamanho      | `stats.size`                                    |
| Datas        | `stats.atime`, `stats.mtime`, `stats.birthtime` |
| Permissões   | `stats.mode`                                    |
| Dono         | `stats.uid`, `stats.gid`                        |
<br>

---

### **`fs.Dirent`**
Resultado leve do **`fs.readdir()`**
### O que é

- Uma **classe** que representa uma **entrada de diretório** (_directory entry_).
- É retornada quando usamos `fs.readdir()` com a opção `{ withFileTypes: true }`.
- Serve apenas para **saber o tipo básico do item** (arquivo, diretório, link, etc.) sem precisar acessar o sistema de arquivos novamente.
- **Não retorna tamanho, datas ou permissões.**

### Exemplo

```js

const fs = require('fs');

fs.readdir('./meus_arquivos', { withFileTypes: true }, (err, arquivos) => {
  if (err) throw err;

  arquivos.forEach((dirent) => {
    console.log(dirent.name);
    console.log('É diretório?', dirent.isDirectory());
    console.log('É arquivo?', dirent.isFile());
  });
});

```
<br>
### O que retorna (`fs.Dirent`)

|Propriedade|Descrição|
|---|---|
|`dirent.name`|Nome do item|
|`dirent.isFile()`|É arquivo|
|`dirent.isDirectory()`|É diretório|
|`dirent.isSymbolicLink()`|É link simbólico|
|`dirent.isSocket()`|É socket|
|`dirent.isBlockDevice()`|É dispositivo de bloco|
|`dirent.isCharacterDevice()`|É dispositivo de caractere|
|`dirent.isFIFO()`|É pipe FIFO|
<br>

----

### **Diferença principal entre `fs.Dirent()` e `fs.stat()`

|Característica|`fs.stat()`|`fs.Dirent`|
|---|---|---|
|Tipo|**Função (método)**|**Classe (objeto retornado)**|
|Fonte|Retorna dados do sistema de arquivos|Vem de `fs.readdir()` com `{ withFileTypes: true }`|
|Faz acesso ao disco?|✅ Sim (mais lento)|❌ Não (mais rápido)|
|Retorna tamanho do arquivo?|✅ Sim (`stats.size`)|❌ Não|
|Retorna datas (criação, modificação)?|✅ Sim|❌ Não|
|Retorna tipo (arquivo/pasta)?|✅ Sim|✅ Sim|
|Ideal para...|Quando precisa de **informações completas**|Quando quer **listar diretórios rapidamente**|
|Objeto retornado|`fs.Stats`|`fs.Dirent`|
<br>
### Exemplo comparativo**

```js

const fs = require('fs').promises;
const path = require('path');

async function comparar() {
  const itens = await fs.readdir('./meus_arquivos', { withFileTypes: true });

  for (const dirent of itens) {
    const caminho = path.join('./meus_arquivos', dirent.name);

    console.log(`📄 Nome: ${dirent.name}`);
    console.log(`É diretório? ${dirent.isDirectory()}`);
    console.log(`É arquivo? ${dirent.isFile()}`);

    // Agora vamos buscar detalhes com fs.stat()
    const stats = await fs.stat(caminho);
    console.log(`  → Tamanho: ${stats.size} bytes`);
    console.log(`  → Criado em: ${stats.birthtime}\n`);
  }
}

comparar();

```
<br>
> 	**`fs.Dirent`** dá as informações rápidas (nome, tipo)
> 	**`fs.stat()`** completa os detalhes (tamanho, datas, etc).

<br>
## Dica de uso profissional

👉 Quando for **listar diretórios grandes**:

- Use `fs.readdir({ withFileTypes: true })` para obter os tipos de arquivo rapidamente.
- Use `fs.stat()` **somente nos itens que você realmente precisa detalhar** (ex: mostrar tamanho de arquivos, mas não de pastas).

Assim, você economiza **muitas chamadas ao disco** e o script fica **muito mais rápido** ⚡.


