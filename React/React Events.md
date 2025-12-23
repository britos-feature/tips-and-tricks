# **Eventos** no React são **funções disparadas** quando algo acontece na interface.

**Exemplo:**

- o usuário **clica** em um botão,
- **digita** em um campo de texto,
- **envia** um formulário,
- **passa o mouse**, etc.

> Eles funcionam de forma **muito parecida com os eventos do JavaScript puro (DOM)**, mas com algumas **diferenças importantes**.
<br>
**Exemplo:**

```jsx

<button onClick={() => alert("Hi, welcome!")}> Click here </button>

```
<br>

## Diferenças entre eventos React e DOM nativo

| DOM Nativo                                    | React                                                                             |
| --------------------------------------------- | --------------------------------------------------------------------------------- |
| Usa letras minúsculas: `onclick`, `onchange`  | Usa **camelCase**: `onClick`, `onChange`                                          |
| Usa strings: `<button onclick="alert('Hi')">` | Usa funções: `<button onClick={handleClick}>`                                     |
| Eventos são reais (DOM)                       | Eventos são _sintéticos_ (**Synthetic Events**) — uma camada do React sobre o DOM |
| Precisa remover manualmente listeners         | React gerencia automaticamente                                                    |


---

## **Synthetic Event**

**_`SyntheticEvent`_** é um **objeto especial do React** que padroniza o comportamento de eventos para todos os navegadores.

> Não importa se você está no Chrome, Firefox ou Safari — o React garante que **`event.target`**, **`event.preventDefault()`** e outras propriedades funcionem da mesma forma.

**Exemplo básico**

```jsx

import React from "react";

function ButtonEvent() {
	
	function handleClick(event) {
	    console.log("Botão clicado!");
	    console.log(event); // mostra o SyntheticEvent (ñ obrigatório)
  }

  return <button onClick={handleClick}>Clique aqui</button>;
}

export default ButtonEvent;

```


**Explicação**

- Quando o botão é clicado, o React dispara `onClick`.
- O React cria um `SyntheticEvent` e passa para a função `handleClick`.
- A função é executada com os dados do evento.
<br>
**Exemplo básico (COM PARÂMETRO)

```jsx 

import React from "react";

function ButtonEvent() {
	
	// Parâmetro "event" e "name" passados para a função
	function handleClick(event, name) {
	    console.log(event); // mostra o SyntheticEvent
	    console.log("Botão clicado!");
	    console.log("Hi, welcome", name);
  }

		// Argumentos "event" e "Jonh" passados a função filho
	return <button onClick={(event) => handleClick(event, "Jonh")}>Clique aqui</button>;
}

export default ButtonEvent;

```

**Importante!**

- Os **_Synthetic Events_** (parâmetro de "event") são passados automaticamente pelo **_`event`_ acionador** a função a ser executa.
	
	**`onClick={handleClick}`**.<br>
- **Exceção**, quando uma função a ser executada pelo **_`event`_ acionador** tiver outro **parâmetro** a ser passado, diferente do **_Synthetic Events_** (parâmetro de "event"), deve-se declarar **explicitamente o parâmetro `event`** a função juntamente com o outro parâmetro, por meio de outra **função**, (normalmente) por **arrow-function**.

	- **`onClick={(event) => handleClick(event, "Jonh")}`**


---


## **Prevenindo (comportamento padrão)**

Assim como no JavaScript puro, podemos usar `preventDefault()` para **prevenção do comportamento padrão de um FORM** ao enviar.

```jsx

function FormExample() {

	function handleSubmit(event) {
	    // Cancelamento comportamento padrão, evita o recarregamento da pagina
	    event.preventDefault(); 
	    console.log("Form enviado!");
	}

	return (
	    <form onSubmit={handleSubmit}> 
			<input type="text" placeholder="Seu nome" />
			<button type="submit">Enviar</button>
	    </form>
	);
}

```
<br>

## **Obtendo dados do "FORM"**

Para se obter dados de um formulário em **JSX REACT** é necessário a criação de um **_state_** (variável de estado), a declaração do **atributo _`value`_** e de algum **evento** que informe a alteração do **_`state`_** , exemplo **_`onChange`_**.

**Exemplo**

```jsx

import React, { useState } from "react";

function FormExample() {
	
	// Criação variável de estado (state)
	const [name, setName] = useState("");

	function handleSubmit(event) {
		// Cancelamento comportamento padrão, evita o recarregamento da pagina
		event.preventDefault();
		console.log("Form enviado!");
		console.log(name);
	}

	// Manipulador do valor de entra "name" do FORM
	function handleChangeValue(e) {
		
		// Método atualizador para "state" (name)	
		setName(() => {
			return e.target.value;
		});
	}

	return (
		// event carregador FORM
		<form onSubmit={handleSubmit}> 
			<input
				type="text"
				value={name} // value para "state" (name)
				onChange={handleChangeValue} // event gatinho para "state"
				placeholder="Insert name"
			/>
			<button type="submit">Enviar</button>
		</form>
	);
};

export default FormExample;

```
<br>
**Resumo:**

- **`useState("")`** → cria uma variável `name` com valor inicial vazio.
- **`setName()`** → atualiza o state.
- **`onChange`** → dispara toda vez que o usuário digita.
- **`onSubmit`** → dispara quando o formulário é enviado.
- **`event.preventDefault()`** → impede o comportamento padrão (recarregar a página).

