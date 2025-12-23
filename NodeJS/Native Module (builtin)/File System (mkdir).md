No **Node.js**,  o método **`fs.mkdir`** do módulo **`fs` (File System).** é utilizado para criar diretórios.
As principais funções são:

- `fs.mkdir()` → assíncrona (**callback**)
- `fs.mkdirSync()` → **síncrona**
- `fs.promises.mkdir()` → baseada em **_Promises_** (ideal para **`async/await`**)

Além disso, podemos usar **opções** como `{ recursive: true }` e **verificar se o diretório já existe**.

### **Utilização `fs.mkdir`**

- ### `fs.mkdirSync()` - Síncrono
	(Modo síncrono)  - Esse modo **bloqueia o código seguinte** espera até que o modo **`fs.mkdir()`** termine de criar seu diretórios.
	
	**Sintaxes: `fs.mkdirSync( path, [options]);`
	
```js

// Síncrono

const fs = require('fs'); 
const path = require('path');

const dirPath = path.join(__dirname, 'meuDiretorioSync');  

try {
	fs.mkdirSync(dirPath);
	console.log('Diretório criado com sucesso!');
}
catch (err) {
	console.error('Erro ao criar diretório:', err);
}

```
<br>
### Utilizando opções

```js

// Síncrono

try {
	fs.mkdirSync(dirPath, { recursive: true, mode: 0o755 });
	console.log('Diretório criado (recursivo)!'); 
}
catch (err) {
	console.error(err); 
}

```
<br>
> **Options:**
> 	- **`recursive: true`** → cria todos os diretórios intermediários caso não existam.
> 	- **`mode`** → define permissões (em octal, ex: `0o755`).

---

- ### **`fs.mkdir()`** - Assíncrono (CallBack)
	(Assíncrono com Callback) - É a forma **clássica** assíncrona, não trava o Node.js, mas usa callbacks.

	**Sintaxes: `fs.mkdir( path, [options], callback);`**

```js

// Assíncrono (callback)

const fs = require('fs');
const path = require('path');

const dirPath = path.join(__dirname, 'meuDiretorioCallback');

fs.mkdir(dirPath, (err) => {
   if (err) {
	   return console.error('Erro ao criar diretório:', err);
   }   console.log('Diretório criado com sucesso!'); 
});`

```
<br>

### Utilizando opções

```js

fs.mkdir(dirPath, { recursive: true, mode: 0o755 }, (err) => {
   if (err) {
	   return console.error(err);
   }
   console.log('Diretório (recursivo) criado!'); 
});`

```
<br>
> **Options:**
> 	- **`recursive: true`** → cria todos os diretórios intermediários caso não existam.
> 	- **`mode`** → define permissões (em octal, ex: `0o755`).


---

- ### **`fs.promises.mkdir()`** - (Assíncrono async - await) (promises)
	(Promises / async - await) - Forma **moderna e mais limpa** de lidar com operações assíncronas.
	
	**Sintaxes: `fs.mkdir( path, [options]);**
	
```js

// Asíncrono (async - await)

const fs = require('fs').promises;
const path = require('path');

async function criarDiretorio() {
	const dirPath = path.join(__dirname, 'meuDiretorioAsync');
	
	try {
		await fs.mkdir(dirPath);
		console.log('Diretório criado com sucesso!');
	}
	catch (err) {
		console.error('Erro ao criar diretório:', err);
	}
}

criarDiretorio();

```
<br>
### Utilizando opções

```js

async function criarDiretorioRecursivo() {
	const dirPath = path.join(__dirname, 'pasta1/pasta2/pasta3');
	
	try {
		await fs.mkdir(dirPath, { recursive: true, mode: 0o755 });
		console.log('Diretórios criados (recursivo)!');
	}
	catch (err) {
		console.error(err);
	}
}

criarDiretorioRecursivo();

```
<br>
> **Options:**
> 	- **`recursive: true`** → cria todos os diretórios intermediários caso não existam.
> 	- **`mode`** → define permissões (em octal, ex: `0o755`).


---

- ### **`fs.promises.mkdir()`** - (Assíncrono then / catch) (promises)

```js

// Assíncrono (then - catch)

const fs = require('fs').promises;

fs.mkdir('./pastaProm', { recursive: true })
	.then(() => console.log('Diretório criado!'))
	.catch(err => console.error('Erro:', err));

```
<br>

---

#### **Extra – Verificar se o diretório já existe antes de criar**

```js

const fs = require('fs');
const path = require('path');

const dirPath = path.join(__dirname, 'exemplo');

if (!fs.existsSync(dirPath)) {
	fs.mkdirSync(dirPath);
	console.log('Diretório criado!');
	}
	else {
		console.log('Diretório já existe!'); 
	}
}`

```
<br>
### **Resumo rápido**

| Modo     | Função                | Bloqueia execução? | Suporte a Promises |
| -------- | --------------------- | ------------------ | ------------------ |
| Síncrono | `fs.mkdirSync()`      | ✅ Sim              | ❌ Não              |
| Callback | `fs.mkdir()`          | ❌ Não              | ❌ Não              |
| Promises | `fs.promises.mkdir()` | ❌ Não              | ✅ Sim              |

---

### **Tratando o caso "diretório já existe"**
Por padrão, se o diretório já existir e `recursive: false`, ocorre erro **`EEXIST`**.

**Soluções:**

- **Usar `recursive: true`** → ignora o erro se já existir.
- **Verificar antes com o método `fs.existsSync()`**

```js

// Método fs.existsSync()

const fs = require('fs'); 
const dir = './teste';  
if (!fs.existsSync(dir)) {
	fs.mkdirSync(dir);   
	console.log('Diretório criado!');
	}
else {
	console.log('Diretório já existe!');
}`

```
<br>

---

### **Boas práticas**

| Situação                         | Recomendação            |
| -------------------------------- | ----------------------- |
| Scripts simples (uma execução)   | `fs.mkdirSync()`        |
| Códigos antigos (callback style) | `fs.mkdir()`            |
| Projetos modernos / async code   | `fs.promises.mkdir()`   |
| Criar árvore de diretórios       | `{ recursive: true }`   |
| Produção / servidor              | Evite métodos síncronos |

---

### **Recomendado** (async + recursivo + seguro)

```js

const fs = require('fs').promises;
const path = require('path');

async function criarDiretorioSeguro(pasta) {
	const dirPath = path.resolve(pasta);    
	
	try {
		await fs.mkdir(dirPath, { recursive: true, mode: 0o755 });
		console.log(`Diretório "${dirPath}" criado com sucesso!`);
	} 
	catch (err) {
		console.error(`Erro ao criar diretório: ${err.message}`);
	}
}

criarDiretorioSeguro('./dados/imagens/perfil');``

```
<br>
