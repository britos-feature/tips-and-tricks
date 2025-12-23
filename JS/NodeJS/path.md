# **`node:path`**

O módulo **`path`** do Node.js é um módulo nativo que fornece utilitários para trabalhar com caminhos de arquivos e diretórios de forma independente do sistema operacional.

## **Principais Funções do `path`**

🔹 **`path.basename(p)`** → Retorna o nome do arquivo a partir do caminho.  
🔹 **`path.dirname(p)`** → Retorna o diretório pai do arquivo.  
🔹 **`path.extname(p)`** → Retorna a extensão do arquivo.  
🔹 **`path.join(...paths)`** → Junta segmentos de caminho automaticamente.  
🔹 **`path.resolve(...paths)`** → Retorna um caminho absoluto.  
🔹 **`path.parse(p)`** → Converte um caminho em um objeto com detalhes.  
🔹 **`path.format(obj)`** → Faz o inverso de `parse`, criando um caminho a partir de um objeto.


#### **`basename()`**

Retorna o nome do diretório | arquivo atual

```js
console.log(path.basename(__dirname)); 
// Exemplo:
// NodeJS
```


#### **`reslove()`**

Retorna o caminho absoluto do diretório | arquivo atual

```js
console.log(path.resolve(__dirname)); // /home/britos/Documents/Curso JScript/codes/nodeJS
console.log(path.resolve(__filename)); // /home/britos/Documents/Curso JScript/codes/nodeJS/index.js

console.log(__dirname); // mesmo resultado que path.resolve()
console.log(__filename); // mesmo resultado que path.resolve()
```


#### **`extname()`**

Retorna a extensão do arquivo atual.

```js
console.log(path.extname(__filename)); // .js
```


#### **`joinn()`**

Junta segmentos de caminho de forma segura, corrigindo separadores automaticamente. */

```js
console.log(path.join(__dirname, "test")); 

// Exemplo:
// /home/britos/Documents/Curso JScript/codes/nodeJS/test
```


#### **`parse()`**

Retorna um objeto com várias partes do caminho.

```js
console.log(path.parse(__filename));

{
  root: '/',
  dir: '/home/britos/Documents/Curso JScript/codes/nodeJS',
  base: 'path.js',
  ext: '.js',
  name: 'path'
} 

let obj = { dir: "/home/Britos/Documents", name: "tutorNodeJS", ext: ".txt" };
```


#### **`format(obj)`**

Monta um caminho a partir de um objeto.

```js
console.log(path.format(obj)); // /home/Britos/Documents/tutorNodeJS.txt
```


#### **`delimiter()`**

Retorna o delimitador de caminhos no sistema (; no Windows, : no Linux/macOS).

```js
console.log(path.delimiter); // ":"
```



#### **`sep()`**

Retorna o separador de diretórios do sistema (\ no Windows, / no Linux/macOS).

```js
console.log(path.sep); // "/"
```



#### **`relative(from, to)`**

Retorna o caminho relativo de **from** para **to.**

```js
console.log(path.relative('/home/user/docs', '/home/user/images/photo.jpg')); 
// '../images/photo.jpg'
```
