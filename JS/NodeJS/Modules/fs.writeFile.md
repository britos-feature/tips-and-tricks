	# FS - File System
Module FS - File System
O módulo fs (File System) do Node.js permite interagir com o sistema de arquivos, possibilitando operações como leitura, escrita, criação, exclusão e manipulação de arquivos e diretórios. Ele fornece métodos **síncronos** e **assíncronos** para essas operações. 


### **`fs.writeFile()`** Modo escrita
No Node.js, o módulo `fs` (File System) permite manipular arquivos e diretórios. O **modo de escrita** do `fs` é usado para gravar ou modificar arquivos de diferentes formas. 


-  **`fs.writeFile()`** Assíncrona (com callBack)
Cria um novo arquivo (ou substitui um existente) e escreve dados nele.

```js
// moduleFS.js
// ( Assíncrono - callback )
const fs = require ("node:fs");

fs.writeFile('file.txt', 'Olá, NodeJS!', {flag: "a"}, (err) => {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log('Arquivo salvo!');
});

// ( Síncrono )
// Método não requer um callback, pois ele bloqueia a execução até que o arquivo seja gravado.
try {
	fs.writeFileSync('file2.txt', 'Olá, Node.js!', { flag: "w", encoding: "utf8" });
	console.log('Arquivo salvo!');
} 
catch (err) {
		console.error(err);
	}
```

**Explicação sobre os parâmetros passados**
- **`file.txt`** => caminho(path) do arquivo que será criado ou substituido
- **`Olá, NodeJS!`** => conteúdo que será escrito/ substituido no arquivo
- **`{ flag: "a" }`** => objeto de configuração **(opçional)**
-  **`(err)`** => function callBack do retorno do método **Assíncrono**


> Se o arquivo já existir, ele será sobrescrito.
> <span style="color: red"><b>IMPORTANTE!</b></span>`fs/promises`, as funções retornam Promises ***não aceitam callback*** como último argumento. 

-  **`fs.writeFile()`** Assíncrona (com promises)

```js
// module.fs
// Assíncrona (promises)
const fs = require("node:fs/promises"); // const fs = require("node:fs").promises;

// Promises Async / Await
async function writeFile() {
    try {
        await fs.writeFile(fullFilePath, "Hello World!", { encoding: "utf-8", flag: "a" });
        console.log("File written successfully");
    } catch (err) {
        console.error("Error writing to file:", err);
    }
}

writeFile();

// or
// Promises then() / catch()
const fs = require("node:fs/promises"); // const fs = require("node:fs").promises;

fs.writeFile(fullFilePath, "Hello World!", { encoding: "utf-8", flag: "a" })
    .then(() => {
        console.log("File written successfully");
    })
    .catch((err) => {
        console.error("Error writing to file:", err);
    });

// Ambos funcionaram com promises!
/*
  requisição module fs/Nodejs, utilizando promises
`require("node:fs/promises");` fs.metodo().then().catch() / Async/ Await
`require("node:fs").promises;` fs.metodo().then().catch() / Async/ Await

`require("fs")`; fs.promises.metodo().then().catch() / Async/ Await
*/
```

> 


**Principais valores para `flag`:**

| Flag    | Descrição                                                                                      |
| ------- | ---------------------------------------------------------------------------------------------- |
| `'w'`   | (Padrão) Abre o arquivo para escrita. Cria o arquivo se não existir ou sobrescreve se existir. |
| `'wx'`  | Como `'w'`, mas falha se o arquivo já existir.                                                 |
| `'a'`   | Abre para anexar dados ao final do arquivo. Cria o arquivo se não existir.                     |
| `'ax'`  | Como `'a'`, mas falha se o arquivo já existir.                                                 |
| `'r+'`  | Abre para leitura e escrita, erro se o arquivo não existir.                                    |
| `'rs+'` | Como `'r+'`, mas tenta abrir o arquivo diretamente ignorando o cache do sistema operacional.   |

**Opções disponíveis para abjeto de configuração:**

| Opção      | Tipo          | Descrição                                                                                     |
| ---------- | ------------- | --------------------------------------------------------------------------------------------- |
| `encoding` | `string`      | Define a codificação do arquivo. Padrão: `'utf8'`. Exemplos: `'utf8'`, `'ascii'`, `'base64'`. |
| `mode`     | `number`      | Define as permissões do arquivo (padrão: `0o666`).                                            |
| `flag`     | `string`      | Especifica como o arquivo será aberto. Padrão: `'w'`.                                         |
| `signal`   | `AbortSignal` | Permite cancelar a operação com um `AbortController`.                                         |


#### Exemplo:  Escrever em um arquivo com a codificação padrão (`utf8`) - não obrigatório declarar, pois ja é padrão.

```js
const fs = require('fs');

fs.writeFile('arquivo.txt', 'Olá, mundo!', (err) => {
  if (err) throw err;
  console.log('Arquivo salvo!');
});
```


#### Escrever com opções personalizadas (codificação e permissões)

```js
fs.writeFile('arquivo.txt', 'Conteúdo', { encoding: 'utf8', mode: 0o644, flag: 'w' }, (err) => {
  if (err) throw err;
  console.log('Arquivo salvo com opções personalizadas!');
});
```

**Explicação sobre `mode`:** O valor de `mode` é um número octal (prefixado com `0o`) que define **quem pode ler, escrever ou executar** o arquivo. Ele segue a mesma lógica dos comandos `chmod` no Linux.

**Estrutura das permissões (`0oXXX`)**

**Cada número representa as permissões para um grupo de usuários:**

| Grupo               | Significado                    |
| ------------------- | ------------------------------ |
| **Dono (Owner)**    | O usuário que criou o arquivo  |
| **Grupo (Group)**   | Outros usuários do mesmo grupo |
| **Outros (Others)** | Todos os outros usuários       |

**Cada grupo pode ter as seguintes permissões:**

| Valor | Permissão       | Significado                                   |
| ----- | --------------- | --------------------------------------------- |
| `4`   | **r** (read)    | Pode ler o arquivo                            |
| `2`   | **w** (write)   | Pode modificar o arquivo                      |
| `1`   | **x** (execute) | Pode executar o arquivo (caso seja um script) |

Os valores são somados para definir múltiplas permissões.
- `0o644` → Dono pode ler e escrever, os outros só podem ler (`rw-r--r--`).
- Esse é o modo padrão da maioria dos arquivos criados no Linux.


#### Exemplo: Cancelando a escrita com `AbortController`

```js
const fs = require('fs');

const controller = new AbortController();
const { signal } = controller;

fs.writeFile('arquivo.txt', 'Teste de cancelamento', { signal }, (err) => {
  if (err) {
    if (err.name === 'AbortError') {
      console.log('A operação de escrita foi cancelada!');
    } else {
      console.error('Erro ao escrever o arquivo:', err);
    }
  } else {
    console.log('Arquivo salvo com sucesso!');
  }
});

// Cancelando a operação antes que seja concluída
controller.abort();
```

**Explicação:**
- Criamos um `AbortController` e extraímos o `signal`.
- Passamos o `signal` na opção do `fs.writeFile`.
- Chamamos `controller.abort()` logo em seguida, cancelando a operação antes da conclusão.
- Se a escrita for cancelada, o erro `AbortError` é tratado no callback.

**Explicação sobre `signal`**: A opção **`signal`** permite cancelar a operação de escrita usando um `AbortController`. Aqui está um exemplo de uso:

### Assíncrona (basic). promises - then/ catch
```js
fs.promises.writeFile('./Module/fs/text.txt', 'Content inserted with promises \n')
    .then(response => {
        return console.log('Sucess in creating!');
    })
    .catch(error => {
        return console.log('Error: ', error);
    });
```


### Assíncrona (basic).promises -  then/ catch (options)
```js
fs.promises.writeFile('./Module/fs/text.txt', 'Content inserted with promises2 \n', { encoding: 'utf8', flag: 'a' })
    .then(response => {
        return console.log('Sucess in creating!');
    })
    .catch(error => {
        return console.log('Error: ', error);
    });
```


### Assíncrona (basic)
```js

async function writeFile() {
    try {
        await fs.promises.writeFile('./Module/fs/text.txt', 'Content inserted with promises3', 'utf8');
        console.log('Sucess');
    }
    catch (e){
        console.log('Deu ruim!')
    }
}

writeFile();
// O arquivo será sobrescrito se já existir. Para adicionar conteúdo sem sobrescrever, use fs.appendFile().
```

### **Vantagens do `fs.promises.writeFile()`**

✅ Usa `async/await`, deixando o código mais **limpo e legível**.  
✅ Facilita o **tratamento de erros** com `try/catch`.  
✅ Não precisa de callbacks aninhados.

