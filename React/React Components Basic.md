
O **React** é uma **biblioteca JavaScript** criada pelo **Facebook (Meta)** para construir **interfaces de usuário (UI)**.  

**UI - (Interface de usuário)**, corresponde **tudo aquilo que o usuário vê e com o que ele interage** em um sistema, site ou aplicativo.

Ele é usado principalmente para **criar aplicações web dinâmicas e reativas**, como painéis, redes sociais, lojas virtuais, etc.

---

### Conceitos Fundamentais do React

| Conceito        | Explicação                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Componentes** | São blocos independentes e reutilizáveis da interface (como peças de Lego). Cada componente controla sua própria estrutura e comportamento. |
| **JSX**         | É uma sintaxe que permite escrever HTML dentro do JavaScript. Exemplo: `<h1>Hello, React!</h1>`                                             |
| **Props**       | São parâmetros passados de um componente pai para um componente filho, parecidos com argumentos de função.                                  |
| **State**       | Guarda os dados internos de um componente. Quando o estado muda, o React atualiza automaticamente a interface.                              |
| **Hooks**       | Funções especiais que permitem “ligar” recursos do React (como o `useState`, `useEffect`, etc.) em componentes funcionais.                  |
| **Virtual DOM** | É uma cópia leve do DOM real que o React usa para aplicar mudanças de forma eficiente e rápida.                                             |
<br>

---

### Sintaxes básica para criação de projeto React ( CRA )

```sh

# initial project react
npx create-react-app myproject

```

> **OBS:.** Nomes de projetos não podem conter caracteres _**maiúsculos.**_
<br>
---

### Estrutura simples criada

```php

meu-projeto/
 ├─ src/
 │   ├─ App.js          # Componente principal
 │   ├─ index.js        # Ponto de entrada da aplicação
 │   └─ components/     # Onde ficam seus componentes
 ├─ public/
 ├─ package.json
 └─ node_modules/

```
<br>

---

# Funcionamento do _React_

O React **não manipula o DOM diretamente**.  
Ele utiliza o conceito de **Virtual DOM** — uma cópia leve do DOM real mantida na memória.

**Fluxo simplificado:
-  Você altera o **estado (state)** de um componente.
-  O React **atualiza o Virtual DOM**.
-  Ele compara (diffing) a nova versão com a anterior.
-  Só aplica as mudanças **necessárias** no DOM real — tornando tudo **mais rápido**.
 <br>
- ##  Components
	
	- São **_funções ou classes_** que sempre precisam  retornarem um elementos de interface, eles podem ser **funcionais (funções)** tipos modernos, utilizados atualmente, ou **de classes** tipo **legado**.

- ##  Conceitos fundamentais

	1. Em um component react **é obrigatório sempre retornar um elemento**.

	2. Para se **retornar mais de um elemento** em um component react, os mesmo devem ser envolvido do **parênteses e outro elemento**
		- `()`
		- `<>`, `<React.Fragment`
		- `<div>`
		- ou qualquer outro elemento que consigo envolver os outros elementos 


---

# Método de criação de _`components`_ basic

- ## Component de **class** (legado)


```jsx

// src/components/Main.jsx
 
import React, { Component } from 'react';

class Main extends Component {
	render(
		return <h1>Hello World!</h1>
	);
} 
 
export default Main;

```
<br>

- ## Component funcionais - "funções" ( modernos atuais )

```jsx

// src/components/Main.jsx

import React from 'react';

function Main () {
	return <h1>Hello World!</h1>
}

export default Main;

```
<br>

---

# Método de criação de _`components`_ utilizando _`state`_

- ## Component de **class** (legado) - **_stateful_**

	- ## Modo 1 (verboso)

```jsx

// src/components/Main.jsx

import React, { Component } from 'react';

class Main extends Component {
	constructor(props) {
		super(props);
		
		this.state = {
			isLogged: false,
		};
	
		this.handleClick = this.handleClick.bind(this);
	}

	handleClick(e) {
		e.preventDefault();
	
		return this.setState((prevState) => ({
			isLogged: !prevState.isLogged,
		}));
	}
	
	render() {
		const { isLogged } = this.state;
	
		return (
			<>
				<h1> Login status: {isLogged ? 'On' : 'Off'}</h1>
				<button type="button" onClick={this.handleClick}>
					Alter
				</button>
			</>
		);
	}
}

export default Main;

```


  - ## Mode 2 "melhorado" - (class fields - stateful), mais ainda (legado)

```jsx

// src/components/Main.jsx

import React, { Component } from 'react';

class Main extends Component {
	// desativar regra eslint (contructor)
	state = {
		isLogged: false,
	};
	
	handleClick = (e) => {
		e.preventDefault();
	
		return this.setState((prevState) => ({
			isLogged: !prevState.isLogged,
		}));
	}
	
	render() {
		const { isLogged } = this.state;
	
		return (
			<>
				<h1> Login status: {isLogged ? 'On' : 'Off'}</h1>
				<button type="button" onClick={this.handleClick}>
					Alter
				</button>
			</>
		);
	}
}

export default Main;

```


> Está **class** é um **component stateful** (ou **componente com estado**), um componente que **possui e gerencia seu próprio estado interno** no React. <br>
> Em outras palavras: ele **guarda informações que podem mudar ao longo do tempo** e, quando esse estado muda, o componente **re-renderiza**.


## O que é “state” no React?

O **state** é um objeto usado para armazenar dados que:

- Mudam com interação do usuário (clique, formulário, etc.)
- Afetam o que é renderizado na tela
- São controlados pelo próprio componente

---

- ## Components funcionais - **stateful** ( modernos atuais )
	
	- ## Component de **_function_** 

```jsx

import { useState } from 'react';

// src/components/Main.jsx

function Main () {
	const [isLogged, setIsLogged] = useState(false);

	return (
		<>
			<h1> Login status: {isLogged ? 'On' : 'Off'}</h1>
			<button type="button" onClick={() => 
				setIsLogged((prevState) => !prevState)}>
				Alter
			</button>
		</>
	);
}

export default Main;
```
<br>
## 📌 Regra de ouro

> Sempre use **`prevState`** quando o novo state depende do state anterior