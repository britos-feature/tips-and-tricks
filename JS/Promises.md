# Promises

Em JavaScript, **Promises** são um recurso **[^1]assíncrono** que representa um valor que pode estar disponível agora, no futuro ou nunca. Elas são muito usadas para operações assíncronas, como requisições a APIs, leitura de arquivos, timers, entre outros.

Uma **Promise** (ou promessa) é um objeto que representa a eventual **conclusão ou falha** de uma operação assíncrona. Quando trabalhamos com operações que **não retornam resultados imediatamente**, como requisições a um servidor, leitura de arquivos ou timers, as Promises ajudam a gerenciar essas operações de maneira mais estruturada.

Uma Promise pode estar em três estados:

- **Pending (Pendente)** – Quando a operação ainda não foi concluída.
- **Fulfilled (Resolvida)** – Quando a operação foi concluída com sucesso.
- **Rejected (Rejeitada)** – Quando a operação falhou.

Ciclo de Vida de uma Promise:

-  Quando uma Promise é criada, ela começa no estado **Pending**.
-  Se a operação for bem-sucedida, a Promise muda para o estado **Fulfilled** e chama a função **`resolve()`**.
-  Se a operação falhar, a Promise muda para o estado **Rejected** e chama a função **`reject()`**.

## **Modos de declaração para Promises**
No JavaScript moderno, existem várias formas de declarar e trabalhar com **Promises**, incluindo **criação, uso e encadeamento**.

### **Declarações**

- #### **Utilizando Construtor para Promises**

```js
main.js

const myPromises = new Promises((resolve, reject) => {
	// simulador de operação assíncrona
	setTimerout(() => {
		const success = true; // altere para "false" para testa "reject"
		if (success) {
			resolve("Operação executada com sucesso!");
		}
		else {
			reject("Error, ocorreu algum error na operação!");
		}
	}, 3000);
});


// consumindo a Promise
myPromises
	.then(response => console.log(response)) // quando 'resolve' for chamado
	.cath(error => console.log(error)) // quando 'reject' for chamado
	.finally(() => console.log("Finalizado!")); // sempre será executado
```


- #### **Usando `async/await` (Sintaxe mais moderna)**
O **`async/await`** permite escrever código assíncrono de forma mais parecida com código síncrono.

**Exemplo 1 **

```js
main.js

async function exampleAsync () {
	try {
		let result = await new Promises( resolve => {
			setTimerout(() => {
				resolve("Operação executada com sucesso!");
			}, 4000);

			console.log(result); // saída dos dados resolvidos
		});
	}
}

exampleAsync(); // chamada da função!
```

**Exemplo 2**

```js
main.js

function esperar(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function executar() {
  console.log("Iniciando...");
  await esperar(2000); // Aguarda 2 segundos
  console.log("Finalizado!");
}

executar();

```


- #### Encadeamento de Promises
Você pode encadear `.then()` para executar várias operações de forma sequencial

```js
new Promise((resolve) => {
  setTimeout(() => resolve(10), 1000);
})
  .then(numero => {
    console.log(numero); // 10
    return numero * 2;
  })
  .then(resultado => {
    console.log(resultado); // 20
    return resultado * 3;
  })
  .then(final => {
    console.log(final); // 60
  });

```
### **Resumo**

- Promises são usadas para gerenciar operações assíncronas.
    
- Possuem três estados: **pending**, **fulfilled** e **rejected**.
    
- Utilizamos `.then()` para capturar sucesso e `.catch()` para erros.
    
- Podemos encadear `.then()` para executar operações sequenciais.
    
- **`async/await`** facilita a escrita de código assíncrono de maneira mais intuitiva.


---
---

[^1]: A palavra **assíncrona** refere-se a um conceito onde a execução de uma tarefa não ocorre de forma simultânea ou em sequência direta, permitindo que outras operações sejam realizadas enquanto uma determinada tarefa está em andamento. Esse termo é amplamente utilizado em programação, especialmente no contexto de operações que envolvem I/O (entrada/saída), como chamadas a APIs, leitura de arquivos ou operações de rede. ***Resumindo:*** Assíncrono é quando uma tarefa é feita sem precisar esperar outra terminar. Na programação, isso deixa o código mais rápido e eficiente.
