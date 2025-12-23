# Global variables
O Node.js disponibiliza várias variáveis globais que podem ser usadas em qualquer parte do código sem a necessidade de importação. 
Aqui estão as principais:

### `__dirname`
Retorna o diretório do arquivo atual.

```js
console.log(__dirname); 

// Exemplo:
// /home/user/projeto (Linux/macOS) 
// C:\Users\user\projeto (Windows)
```


### **`__filename`**

Retorna o caminho completo do arquivo atual.

```js
console.log(__filename); 
// Exemplo:
// /home/user/projeto/app.js (Linux/macOS)
// C:\Users\user\projeto\app.js (Windows)
```


### **`process`**
É um objeto que fornece informações sobre o processo em execução. 

```js
console.log(process.pid); // ID do processo
console.log(process.cwd()); // Diretório de trabalho atual
console.log(process.platform); // Plataforma do SO (win32, linux, darwin)
console.log(process.env); // Variáveis de ambiente
```


### **`global`**
Um Objeto global no Node.js (equivalente ao windows no navegador).

```js
global.minhaVariavel = 'Hello, World!';
console.log(global.minhaVariavel); // 'Hello, World!'
```


### **`setTimeout(callBack, delay)`**
Executa uma função após um tempo determinado.
Essa função agenda a execução de uma função (callback) após um determinado tempo (delay) em milissegundos.  Ela é assíncrona e não bloqueia a execução do código.

```js
setTimeout(() => {
    console.log('Executado após 2 segundos');
}, 2000);
```


### **`setInterval(callback, delay)`**
Executa uma função repetidamente a cada intervalo de tempo. 
Essa função executa repetidamente uma função (callback) a cada intervalo de tempo (delay em milissegundos), até que seja explicitamente interrompida.

```js
setInterval(() => {
    console.log('Executando a cada 3 segundos');
}, 3000);

// O clearInterval(interval) interrompe a execução contínua do setInterval.
```


### **`setImmediate(callback)`**
Executa uma função imediatamente após a fase de I/O da event loop. 
Essa função executa o callback assim que possível, após a fase de I/O (Input/Output) da event loop, antes da próxima iteração do loop.

> Diferença entre **setImmediate** e **setTimeout**:
> -  setImmediate(callback): Executa depois da fase de I/O.
> -  setTimeout(callback, 0): Executa após um pequeno atraso mínimo. 

```js
setImmediate(() => {
    console.log('Executado imediatamente após o I/O');
});
```


### **`clearTimeout(timeoutId)`**
Cancela um setTimeout. */

```js
const timeout = setTimeout(() => console.log('Não será executado'), 5000);
clearTimeout(timeout);
```


### **`clearInterval(intervalId)`**
Essa função cancela um **setInterval()**, impedindo que ele continue executando.

```js
const interval = setInterval(() => console.log('Loop infinito'), 1000);
clearInterval(interval);
```
