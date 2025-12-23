O módulo **`fs` (File System)** do Node.js é um dos módulos **core** do Node.js, ou seja, já vem instalado por padrão, sem precisar de instalação adicional. Ele fornece **funções para interagir com o sistema de arquivos**, como ler, escrever, renomear, deletar arquivos e diretórios.
Ele fornece métodos síncronos e assíncronos para essas operações:
fs.readdir, fs.readfile, fs.writefile, fs.mkdir, fs.stat, etc...
<br>
<span style="color:red"><big><b>IMPORTANTE:</b></big></span>  

> 	O método **`fs.writeFile()`** só serve para criar ou sobrescrever arquivos.
> 	 Ao tentar executar o módulo **`fs.writeFile()`** em um **diretório que ainda não existe**, vai gerar um erro **(`ENOENT: no such file or directory`)**.  Então, para criar um arquivo em um **novo diretório** utilize o módulo `fs.mkdir()`,  com a opção **`{ recursive: true }`**.
<br>
### Método **`fs.writeFile()`** - Modo de gravar dados em arquivos

O módulo **`fs` (File System)** do Node.js permite interagir com o sistema de arquivos, e uma das Métodos/Função mais usadas é o **`fs.writeFile()`**.

O **`fs.writeFile`** é um método assíncrono e serve para:
	- **Cria o arquivo do zero.**
	- **Sobrescreve o conteúdo de um arquivo existente**.
	- **Gravar texto ou dado binários.
<br>
#### Sintaxes básica
Possui 3 variantes me NodeJS

- **Callback**
	- `fs.writeFile(path, data, [options], callback)
- **Promises**
	- `fs.writeFile(path, data, [options])
- **Síncrona** _(não recomendada para servidores, apenas scripts simples)_
	- `fs.writeFile(path, data, [options])
<br>
#### Parâmetros

- **`path`**: caminho do arquivo (string ou `Buffer` ou `URL`).

- **`data`**: conteúdo que será escrito (pode ser _string_ ou _Buffer_).

- **`options`** (opcional, objeto ou string):
    - `encoding` → padrão: `'utf8'`. Pode ser `'ascii'`, `'base64'`, etc.
    
    - `mode` → define permissões do arquivo (padrão: `0o666`, ou seja, leitura e escrita para todos).

    - `flag` → define como abrir o arquivo:
        - **`'w'`** write (default) **→** Cria novo de não existir, se existir sobrescreve.
        - **`'wx'`** write exclusive **→**  Cria o arquivo **somente se não existir**. Se já existir, lança erro.
        - **`w+`** write, read **→** Igual a `'w'`, mas também permite leitura.
        - **`wx+`** write exclusive, read **→** Cria apenas se não existir e também abre para leitura.

        - **`'a'`** append **→** Cria o arquivo se não existir. Se existir, adiciona conteúdo no final.
        - **`'ax'`** append exclusive **→** Igual a `'a'`, mas só funciona se o arquivo **não existir**.
        - **`a+`** append, read **→** Igual a `'a'`, mas também abre para leitura.
        - **`ax+`** append exclusive, read **→** Igual a `'ax'`, mas também abre para leitura.

- **`callback(err)`**: função chamada após a escrita.
    - Se `err` for `null`, deu tudo certo.

    - Se `err` existir, algo falhou (ex: permissão negada, disco cheio, etc.).
<br>
### **Exemplos Básicos sem Options:**

- **Usando version Síncrona** (`fs.writeFileSync`)
	Ideal para scripts pequenos, mas não recomendado em servidores porque **bloqueia o event loop** até terminar a escrita.

```js
// SÍNCRONA

const fs = require('fs');

try {
  fs.writeFileSync('sincrono.txt', 'Conteúdo síncrono gravado!');
  console.log('Arquivo salvo (writeFileSync).');
} 
catch (err) {
  console.error('Erro ao escrever arquivo síncrono:', err);
}

```

> ➡ O código só continua **depois** que a gravação terminar.
<br>
---

- **Usando Callback** ( `fs.writeFile` )
	É a forma **clássica** assíncrona, não trava o Node.js, mas usa callbacks.

```js
// CALLBACK

const fs = require('fs');

fs.writeFile('callback.txt', 'Conteúdo com callback!', (err) => {
  if (err) {
    console.error('Erro ao escrever arquivo com callback:', err);
    return;
  }
  console.log('Arquivo salvo (writeFile com callback).');
});

```

> ➡ Enquanto o arquivo é escrito, o Node.js pode continuar executando outras coisas.
<br>
---

- **Usando Promises** ( `fs.promises.writeFile` )
	A forma **mais moderna**: permite usar `async/await` e evita "callback hell".

```js
// PROMISES

const fs = require('fs').promises;

async function escreverArquivo() {
  try {
    await fs.writeFile('promises.txt', 'Conteúdo com Promises!');
    console.log('Arquivo salvo (writeFile com Promises).');
  } catch (err) {
    console.error('Erro ao escrever arquivo com Promises:', err);
  }
}

escreverArquivo();

```

> ➡ Recomendado para projetos atuais, pois combina bem com `async/await`. 
<br>
---

- **Escrevendo em formato JSON**

```js
const fs = require('fs').promises;

async function salvarJSON() {
  const dados = { nome: "Lucas", idade: 25 };

  try {
    await fs.writeFile('dados.json', JSON.stringify(dados, null, 2));
    console.log('Arquivo JSON salvo!');
  } catch (err) {
    console.error(err);
  }
}

salvarJSON();
```

> ➡ `JSON.stringify(obj, null, 2)` formata com indentação.
<br>
---
---

#### **Exemplos Básicos utilizando Options:**

As opções mais comuns são:
	- **`encoding`** → formato do texto (`'utf8'`, `'ascii'`, `'base64'` etc.).
	- **`flag`** → modo de abertura (`'w'` sobrescreve, `'a'` adiciona, `'wx'` cria somente se não existir).
	- **`mode`** → permissões do arquivo (ex.: `0o666` leitura/escrita para todos).
<br>
- **Version Assíncrona**

 ```js
 const fs = require('fs');

try {
  fs.writeFileSync(
    'sincrono.txt',
    'Conteúdo síncrono com opções!\n',
    { encoding: 'utf8', flag: 'w', mode: 0o644 }
  );
  console.log('Arquivo salvo (síncrono com opções).');
} catch (err) {
  console.error('Erro:', err);
}
```

> ➡ **Options:**
> 	- **`utf8`** garante que o texto será gravado em **UTF-8.**
> 	- **`flag: 'w'`** sobrescreve o arquivo.
> 	- **`mode: 0o644`** define permissões (leitura para todos, escrita só para o dono).
<br>
---

- **Version Callback

```js
const fs = require('fs');

fs.writeFile(
  'callback.txt',
  'Conteúdo com callback e opções!\n',
  { encoding: 'utf8', flag: 'a', mode: 0o600 },
  (err) => {
    if (err) {
      console.error('❌ Erro ao escrever (callback):', err);
      return;
    }
    console.log('✅ Arquivo salvo (callback com opções).');
  }
);
```

> **Options:**
> 	- `flag: 'a'` → adiciona no final (append).
> 	- `mode: 0o600` → só o dono pode ler e escrever.
<br>
---

- **Version Promises**

```js
const fs = require('fs').promises;

async function escreverArquivo() {
  try {
    await fs.writeFile(
      'promises.txt',
      'Conteúdo com Promises e opções!\n',
      { encoding: 'utf8', flag: 'wx', mode: 0o644 }
    );
    console.log('Arquivo salvo (Promises com opções).');
  } catch (err) {
    console.error('Erro ao escrever (Promises):', err);
  }
}

escreverArquivo();
```

> **Options**
> 	- `flag: 'wx'` → só cria o arquivo se ele **não existir**, caso contrário dá erro (protege contra sobrescrita).
> 	- **`mode: 0o644`** define permissões (leitura para todos, escrita só para o dono).
<br>
### **Método `fs.appendFile()`**

O método/ função **`fs.appendFile()`** serve para **acrescentar conteúdo** no fim de um arquivo, fazer (**append**) criando o arquivo caso não exista.
<br>
### Diferença entre `fs.writeFile` e `fs.appendFile`

- **`writeFile`** → sobrescreve o arquivo (ou cria, se não existir). 
- **`appendFile`** → adiciona ao final do arquivo sem apagar o que já existe.

|Função|O que faz|Cria arquivo se não existir?|Sobrescreve conteúdo existente?|Adiciona ao final?|Estilo de uso|
|---|---|---|---|---|---|
|**`fs.writeFile`**|Escreve dados em um arquivo|✅ Sim|✅ Sim (apaga o anterior)|❌ Não|Assíncrono (callback)|
|**`fs.writeFileSync`**|Versão síncrona de `writeFile`|✅ Sim|✅ Sim|❌ Não|Bloqueante (trava o fluxo)|
|**`fs.promises.writeFile`**|Versão com `Promise`/`async-await`|✅ Sim|✅ Sim|❌ Não|Assíncrono (await)|
|**`fs.appendFile`**|Adiciona dados no final do arquivo|✅ Sim|❌ Não|✅ Sim (sempre ao final)|Assíncrono (callback)|
|**`fs.appendFileSync`**|Versão síncrona de `appendFile`|✅ Sim|❌ Não|✅ Sim|Bloqueante|
|**`fs.promises.appendFile`**|Versão com `Promise`/`async-await`|✅ Sim|❌ Não|✅ Sim|Assíncrono (await)|
<br>
**Resumindo:**
- Use **`appendFile`** para _não sobrescrever_ dados.
- Se quiser segurança para não sobrescrever um arquivo existente, use **`flag: 'ax'`**.
- Para logs e históricos, é a forma ideal.