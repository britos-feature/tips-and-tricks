 O módulo **`fs` (File System)** do Node.js é um dos módulos **core** do Node.js, ou seja, já vem instalado por padrão, sem precisar de instalação adicional. Ele fornece **funções para interagir com o sistema de arquivos**, como ler, escrever, renomear, deletar arquivos e diretórios.
Ele fornece métodos síncronos e assíncronos para essas operações:
fs.readdir, fs.readfile, fs.stat, fs.writefile, fs.mkdir, etc...
<br>
### Método `fs.readdir()` - Modo de leitura a diretório

O método **`fs.readdir()`** do Node.js pertence ao módulo **`fs` (File System)** e é utilizado para ler o conteúdo de um diretório. Ele retorna uma lista com os nomes dos arquivos e subdiretórios presentes no diretório especificado. 

Dependendo da forma de uso, pode retornar **apenas os nomes (strings)** ou **objetos `fs.Dirent`** que permitem inspecionar cada entrada em mais detalhes.


#### **Sintaxes:**

 - **Versão com Callback (assíncrona):**
 
```js
fs.readdir(path, options, callback);
```
<br>
- **Versão Síncrona:

```js
fs.readdirSync(path[, options])
```
<br>
- **Versão como Promises**:

```js
fs.promises.readdir(path[, options])
```
<br>
#### **Exemplos:**

**básico (callback)**

```js

const fs = require("fs");

fs.readdir("./myFolder", (err, files) => {
	if (err) throw err;
	return "Arquivos no diretório: ", files;
});

```

**Saída possível**

```js

[ 'arquivo1.txt', 'arquivo2.txt', 'subpasta' ]

```
<br>
**básico (síncrono)

```js
const fs = require("fs");

const files = fs.readdirSync("./myFolder");
console.log(files);

```
<br>
**Promises `async - await`**

```js

// async - await
const fs = require("fs").promises;

async function readdir(rootDir) {
	try{
		rootDir = rootDir || path.resolve(__dirname);
		const files = await readdir(rootDir)
		return files
	}
	catch(err) {
		return err
	}
}
readdir("./myFolder");

```
<br>
**Promises `then - catch`**

```js
const fs = require("fs");
// or
const fs = require("fs").promises;

fs.promises.readdir("./myFolder")
	.then((files) => {
			console.log(files);
	})
	.catch((err) => {
		console.log(err);
	});

```
<br>
**Explicação**
- O código lê o conteúdo do diretório **`./myFolder`**.
- Se houver erro, ele será tratado no **`if (err)`**.
- Caso contrário, **`files`** conterá um array de strings com os nomes dos arquivos e pastas do diretório.
<br>
#### **Exemplos utilizando Options:**

O método **`fs.readdir`** (e sua versão síncrona `fs.readdirSync`) aceita um **caminho** e uma **opção opcional**.

**Detalhamento Options:**

- **`path`** (string ou Buffer ou URL ou integer): O caminho do diretório a ser lido.

- **`options`** (opcional, podendo ser um **Type: `string`** ou um **Dirent: `objeto.`**
    - **`encoding`** (padrão: `'utf8'`) **→** codificação dos nomes retornados.
    
    - **`withFileTypes`** (`boolean`) **→** se `true`, retorna objetos `fs.Dirent` em vez de apenas strings.
    
    - **recursive** (`boolean`) **→** (Somente em **Node 20+**) se `true`, percorre o diretório **recursivamente**, retornando também subpastas.
    
    - **signal** (`undefined`) **→** Permite cancelar a operação assíncrona de leitura (útil em streams e controle de abortos).
    
- **`callback`** (função): A função chamada quando a operação é concluída. Recebe dois argumentos:
    - **`err`**: Um erro caso ocorra algum problema.
    
    - **`files`**: Um array contendo os nomes dos arquivos e diretórios dentro do diretório especificado.
<br>
#### **Exemplo especificando Codificação (`encoding`)**
Define a codificação das strings retornadas.

```js
fs.readdir('./meuDiretorio', { encoding: 'utf8' }, (err, files) => {
  if (err) throw err;
  console.log('Lista de arquivos:', files);
});

```
<br>
**Retornado Object (`withFileTypes`)**
Opção que retorna Objetos do tipo **`Dirent`**, que representam cada entrada do diretório e fornecem métodos úteis para determinar o tipo de cada item.

```js
fs.readdir('./meuDiretorio', { withFileTypes: true }, (err, files) => {
  if (err) throw err;
  
  files.forEach(file => {
    console.log(`${file.name} - ${file.isDirectory() ? 'Diretório' : 'Arquivo'}`);
  });
});
```
<br>
#### **Explicação sobre Object `Dirent`**

**DIRENT** é um objeto que representa uma entrada em um diretório lido por funções como **`fs.readdir`** ou  **`fs.readdirSync`**, quando a opção **`{ withFileTypes: true }`** é especificada.

**DIRENT** fornece informações sobre o tipo de entrada encontrada como: arquivo, diretório, link simbólico, etc, e permite manipulações baseadas nessas informações.

**Principais métodos de `Dirent`:**

- **`.isFile():`** Retorna true se a entrada for um arquivo.
- **`.isDirectory():`** Retorna true se a entrada for um diretório.
- **`.isBlockDevice():`** Retorna true se a entrada for um dispositivo de bloco.
- **`.isCharacterDevice():`** Retorna true se a entrada for um dispositivo de caractere.
- .**`isSymbolicLink():`** Retorna true se a entrada for um link simbólico.
- **`.isFIFO():`** Retorna true se a entrada for um pipe (FIFO).
- .**`isSocket()`**: Retorna true se a entrada for um socket.
<br>
#### **Exemplos de utilização** `dirent`

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
<br>
 > **`Dirent`** é útil quando você deseja obter informações sobre o tipo de cada entrada sem precisar chamar fs.stat ou fs.lstat separadamente para cada item, o que pode ser mais eficiente.
<br>
#### **`Usando `fs.promises.readdir()` (Async/Await)`**
Para código assíncrono mais moderno e legível, utilize a versão baseada em `Promises`:

**Vantagens**:

- Evita o uso de callbacks aninhados (callback hell).
- Permite o uso de `try/catch` para tratamento de erros.
- Melhor integração com `async/await`.
<br>
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
<br>
#### **Significados dos erros Comuns**

```js

// diretório inexistene
Error: ENOENT: no such file or directory, scandir 'caminho/invalido'

// Verifique se o diretório existe antes de tentar ler.
Error: EACCES: permission denied, scandir 'diretorio/protegido'
// Error: EACCES: permission denied, scandir 'diretorio/protegido'

// **Número excessivo de arquivos**:
// Se houver muitos arquivos, a execução pode ser lenta. Use `fs.readdir()` em conjunto com streaming se necessário.
	```
<br>
