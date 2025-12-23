# FS - File System
Module FS - File System
O módulo fs (File System) do Node.js permite interagir com o sistema de arquivos, possibilitando operações como leitura, escrita, criação, exclusão e manipulação de arquivos e diretórios. Ele fornece métodos **síncronos** e **assíncronos** para essas operações. 


### **`fs.readFile()`** Modo de leitura (arquivos)
O **`fs.readFile()`** é um método do Node.js usado para **ler o conteúdo de um arquivo** de forma assíncrona. O módulo **`fs`**, que significa "File System" (Sistema de Arquivos), fornece APIs para interagir com o sistema de arquivos, e o **`readFile()`** é um dos métodos principais.


#### Parâmetros:

- **caminho** (path): O caminho para o arquivo que você quer ler. Pode ser uma string, `Buffer`, ou `URL`.
- **opções** (options): Um parâmetro opcional para definir a codificação (por exemplo, 'utf8') ou outras opções. Se não for fornecido, o retorno será um `Buffer`.
    - A codificação pode ser `utf8`, `base64`, ou outros formatos.
    - Se você não especificar uma codificação, o retorno será um `Buffer` (um tipo de dado binário).
- **callback**: A função de retorno de chamada (callback) que recebe dois argumentos:
    - **err**: O erro que ocorreu (se houver).
    - **data**: O conteúdo lido do arquivo. Se a codificação for especificada, `data` será uma string. Caso contrário, será um `Buffer`.


### Exemplos:
### Leitura Assíncrona com Codificação
Neste exemplo, vamos ler um arquivo de texto com codificação UTF-8 e exibir seu conteúdo.

```js
const fs = require('fs');

// Lê o arquivo 'exemplo.txt' com codificação 'utf8'
fs.readFile('exemplo.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Erro ao ler o arquivo:', err);
    return;
  }
  console.log('Conteúdo do arquivo:', data);
});
```


#### Detalhamento:

- **fs.readFile('exemplo.txt', 'utf8', callback)**: Aqui estamos lendo o arquivo chamado `exemplo.txt` e pedindo para o Node.js interpretá-lo como uma string codificada em UTF-8.
- Se ocorrer um erro (por exemplo, o arquivo não existir), o erro será capturado no parâmetro `err` e exibido no console.
- Se o arquivo for lido com sucesso, o conteúdo será armazenado em `data` e exibido no console.


### Leitura Assíncrona Sem Codificação (Retorno em Buffer)
Neste exemplo, vamos ler um arquivo sem especificar uma codificação, o que resultará em um `Buffer` no lugar de uma string.

```js
const fs = require('fs');

// Lê o arquivo 'exemplo.bin' sem especificar a codificação
fs.readFile('exemplo.bin', (err, data) => {
  if (err) {
    console.error('Erro ao ler o arquivo:', err);
    return;
  }
  console.log('Conteúdo do arquivo em Buffer:', data);
});
```

#### Detalhamento:

- **fs.readFile('exemplo.bin', callback)**: O arquivo é lido sem especificar a codificação. Como resultado, o Node.js retorna um `Buffer`, que é uma representação binária dos dados.
- Você pode manipular esse `Buffer` para realizar outras operações, como conversão de dados ou gravação em outro arquivo.


### Erro ao Ler o Arquivo
Neste exemplo, vamos mostrar o comportamento quando um erro ocorre durante a leitura do arquivo (por exemplo, se o arquivo não existir).

```js
const fs = require('fs');

// Tentando ler um arquivo que não existe
fs.readFile('arquivo_inexistente.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Erro ao ler o arquivo:', err);
    return;
  }
  console.log('Conteúdo do arquivo:', data);
});
```


#### Detalhamento:

- **fs.readFile('arquivo_inexistente.txt', 'utf8', callback)**: Neste caso, como o arquivo `arquivo_inexistente.txt` não existe, o parâmetro `err` conterá informações sobre o erro (como o código de erro "ENOENT" indicando que o arquivo não foi encontrado).
- O erro é capturado e mostrado no console.


### Usando Promessas com `fs.promises.readFile`
A partir do Node.js 10, a API `fs.promises` oferece uma versão baseada em promessas dos métodos do `fs`. Isso permite usar `async/await` para tornar o código mais legível.
#### Código com Promessa (Usando `async/await`):

```js
const fs = require('fs').promises;

async function lerArquivo() {
  try {
    const data = await fs.readFile('exemplo.txt', 'utf8');
    console.log('Conteúdo do arquivo:', data);
  } catch (err) {
    console.error('Erro ao ler o arquivo:', err);
  }
}

lerArquivo();
```


#### Detalhamento:

- **fs.promises.readFile('exemplo.txt', 'utf8')**: Aqui estamos usando a versão baseada em promessas do método **`readFile`**, que retorna uma promessa que resolve com o conteúdo do arquivo.
- Usamos o **`async/await`** para tornar o código mais limpo, evitando o uso de callbacks. Caso haja um erro, ele será tratado pelo `catch`.


### Leitura de Vários Arquivos Assíncronos
Se você precisa ler vários arquivos ao mesmo tempo, o `fs.readFile()` pode ser muito útil, já que ele não bloqueia a execução de outras tarefas.

```js
const fs = require('fs');

function lerVariosArquivos() {
  const arquivos = ['exemplo1.txt', 'exemplo2.txt', 'exemplo3.txt'];

  arquivos.forEach(arquivo => {
    fs.readFile(arquivo, 'utf8', (err, data) => {
      if (err) {
        console.error(`Erro ao ler o arquivo ${arquivo}:`, err);
        return;
      }
      console.log(`Conteúdo de ${arquivo}:`, data);
    });
  });
}

lerVariosArquivos();
```

#### Detalhamento:

- **fs.readFile()** é chamado para cada arquivo na lista. Como é uma operação assíncrona, o Node.js pode continuar executando outras tarefas enquanto aguarda a leitura dos arquivos, sem bloquear o restante do código.


## Buffer
No Node.js, um `Buffer` é uma estrutura de dados usada para armazenar e manipular **dados binários** de forma eficiente. Ele permite que o JavaScript trabalhe com **bytes brutos**, algo essencial para operações como leitura e escrita de arquivos, manipulação de imagens e comunicação via rede.

**Motivo da Existência:**  
O JavaScript tradicionalmente trabalha apenas com strings para manipulação de texto, mas não tem suporte nativo para lidar diretamente com dados binários. O `Buffer` resolve essa limitação, permitindo armazenar e modificar bytes diretamente.

> Em **`NodeJS`** podemos criar um **`Buffer`** de várias formas.


### Criando um `Buffer`

#### **Criando um `Buffer` de um Tamanho Específico**
Você pode criar um `Buffer` com um tamanho específico, que será preenchido com valores zero por padrão.

```js
const buffer1 = Buffer.alloc(10);  // Cria um Buffer com 10 bytes, todos 0
console.log(buffer1);  // <Buffer 00 00 00 00 00 00 00 00 00 00>
```

**`Buffer.alloc(size)`**: Cria um `Buffer` de um tamanho especificado e preenche todos os bytes com `0`. Pode ser útil quando você deseja garantir que o conteúdo do buffer não tenha dados residuais.


#### **Criando um `Buffer` a partir de um Array ou Array-like Object**
Você também pode criar um `Buffer` a partir de um array ou objeto tipo array.

```js
const buffer2 = Buffer.from([1, 2, 3, 4, 5]);
console.log(buffer2);  // <Buffer 01 02 03 04 05>
```

**`Buffer.from(array)`**: Cria um `Buffer` a partir de um array de bytes.


#### **Criando um `Buffer` a partir de uma String**
É possível criar um `Buffer` a partir de uma string. Você também pode especificar a codificação da string (por exemplo, 'utf8', 'base64').

```js
const buffer3 = Buffer.from('Olá, Mundo!', 'utf8');
console.log(buffer3);  // <Buffer 4f 6c 61 2c 20 4d 75 6e 64 6f 21>
```

**`Buffer.from(string, encoding)`**: Cria um `Buffer` a partir de uma string, usando a codificação fornecida.


> Não é possível criar diretamente um `Buffer` a partir de um **objeto JavaScript** (`{}`), porque um `Buffer` só pode armazenar **dados binários**. No entanto, você pode converter um objeto em uma string (por exemplo, JSON) e então criar um `Buffer` a partir dessa string.


#### **Alternativa: Criar um Buffer de Propriedades Específicas**
Se quiser armazenar apenas valores específicos do objeto (por exemplo, um array de números), pode criar um `Buffer` assim:

```js
const obj = { valores: [65, 66, 67] };  // Representação de 'ABC'

// Criar Buffer a partir do array numérico
const buffer = Buffer.from(obj.valores);

console.log(buffer.toString());  // 'ABC'
```



### **Lendo e Manipulando Buffers**

#### **Acessando bytes do Buffer**
Você pode acessar os bytes de um `Buffer` como se fosse um array.

```js
const buffer = Buffer.from('ABC');
console.log(buffer[0]);  // 65 (representação ASCII de 'A')
console.log(buffer[1]);  // 66 ('B')
console.log(buffer[2]);  // 67 ('C')
```

> Os valores retornados representam os **códigos ASCII** dos caracteres.


#### **Convertendo Buffer para String**
Se você quiser transformar um `Buffer` de volta para texto, pode usar `.toString()`.

```js
const buffer = Buffer.from('Olá, Mundo!', 'utf8');
console.log(buffer.toString());  // Olá, Mundo!
```

> Você pode passar uma codificação como **`'utf8'`, `'hex'`, `'base64'.`**


#### **Concatenando Buffers**
Para juntar dois buffers, use `Buffer.concat()`.

```js
const buffer1 = Buffer.from('Olá, ');
const buffer2 = Buffer.from('Mundo!');
const buffer3 = Buffer.concat([buffer1, buffer2]);
console.log(buffer3.toString());  // Olá, Mundo!
```

> O método `concat` é útil quando você precisa combinar dados binários.



#### **Convertendo Buffers para Diferentes Formatos**
Buffers podem ser convertidos para **Base64**, **Hexadecimal** e outros formatos.

```js
const buffer = Buffer.from('Node.js');
console.log(buffer.toString('base64'));  // Tm9kZS5qcw==
console.log(buffer.toString('hex'));  // 4e6f64652e6a73
```

> **Base64** é usado para transmitir dados binários como texto (ex.: imagens em JSON).



### **Exemplos Práticos**

#### **1. Lendo um arquivo como Buffer**
Você pode usar o `fs.readFile()` para ler arquivos como um `Buffer`.

```js
const fs = require('fs');

fs.readFile('exemplo.txt', (err, buffer) => {
  if (err) throw err;
  console.log(buffer);  // Exibe os dados binários do arquivo
  console.log(buffer.toString());  // Converte para string
});
```


#### **2. Escrevendo um Buffer em um arquivo**
Podemos gravar um `Buffer` diretamente em um arquivo.

```js
const fs = require('fs');
const buffer = Buffer.from('Conteúdo binário aqui!');

fs.writeFile('saida.txt', buffer, (err) => {
  if (err) throw err;
  console.log('Arquivo gravado!');
});
```


#### **Resumo**

| Operação                               | Código                        |
| -------------------------------------- | ----------------------------- |
| Criar um Buffer vazio                  | `Buffer.alloc(10)`            |
| Criar um Buffer a partir de um array   | `Buffer.from([65, 66, 67])`   |
| Criar um Buffer a partir de uma string | `Buffer.from('Olá')`          |
| Ler um byte do Buffer                  | `buffer[0]`                   |
| Modificar um byte                      | `buffer[1] = 100`             |
| Converter para String                  | `buffer.toString()`           |
| Concatenar Buffers                     | `Buffer.concat([buf1, buf2])` |
| Converter para Base64                  | `buffer.toString('base64')`   |
