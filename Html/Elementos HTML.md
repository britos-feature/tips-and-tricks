	# Selecionando propriedades de elemento anterior no HTML

Se você quer, **no HTML (ou JS)**, pegar propriedades de um elemento **antecessor** (pai ou anterior), pode usar o **DOM**.

<h3>Obtendo element <span style="color:red"><i>PAI</i></span></h3>

```js

const input = document.querySelector("input");
const parent = input.parentElement; // retorna o elemento pai
console.log(parent.tagName);

```
<br>
<h3>Obtendo element <span style="color:red"><i>IRMÃO ANTERIOR</i></span></h3>

```js

	const input = document.querySelector("input");
	const previous = input.previousElementSibling; // retorna o elemento anterior
	console.log(previous.textContent);

```
<h3>Subir mais níveis <span style="color:red"><i>ANCESTRAIS</i></span></h3>

```js

const input = document.querySelector("input");
const grandparent = input.closest("div"); // encontra o ancestral mais próximo <div>
console.log(grandparent);

```

