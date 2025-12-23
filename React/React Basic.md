O **React** é uma **biblioteca JavaScript** criada pelo **Facebook (Meta)** para construir **interfaces de usuário (UI)**.  
Ele é usado principalmente para **criar aplicações web dinâmicas e reativas**, como painéis, redes sociais, lojas virtuais, etc.

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
### Sintaxes básica para criação de projeto React (criação de projeto)

```sh

# initial project react
npx create-react-app myproject

```

> **OBS:.** Nomes de projetos não podem conter caracteres _**maiúsculos.**_
<br>
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

### Funcionamento do _React_

O React **não manipula o DOM diretamente**.  
Ele utiliza o conceito de **Virtual DOM** — uma cópia leve do DOM real mantida na memória.

**Fluxo simplificado:
-  Você altera o **estado (state)** de um componente.
-  O React **atualiza o Virtual DOM**.
-  Ele compara (diffing) a nova versão com a anterior.
-  Só aplica as mudanças **necessárias** no DOM real — tornando tudo **mais rápido**.

 <br>
##  Conceitos principais

- #### Componentes
	- São funções ou classes que retornam elementos de interface. Podem ser **funcionais** (modernos) ou **de classe** (legado).

 ```js
// Basic (moderno(function))

import React from "react";

function Greeting() {
	return <h1>Hello World!</h1>;
}

export default Greeting;


// Basic (legado(class))

import React, { Component } from "react";

class Greeting extends Component {
	render() {
	    return <h1>Hello World!</h1>;
	}
}

export default Greeting;

```
<br>

**Características**:
	**Funcionais (moderno)
	
- Usam Hooks (useState, useEffect, etc.) para lidar com estado e ciclo de vida.
- São mais curtos e fáceis de ler.
- Não precisam de this.
- É o padrão atual do React (**_recomendado pela documentação oficial_**).

	**Class (legado)

- Usam this.state e this.setState() para gerenciar estado.
- Possuem métodos de ciclo de vida, como:
	- componentDidMount()
	- componentDidUpdate()
	- componentWillUnmount()
- São mais verbosos e menos usados atualmente.
<br>
---

- #### JSX (JavaScript XML)
	- Permite misturar **HTML com JavaScript** dentro do mesmo código. _JSX é convertido em chamadas `React.createElement()` que produzem a árvore de elementos da interface._

```jsx
// Basic

import React from "react";

function Greeting() {
	const name = "Mary";
	
	return <h1>Hi, {name}!</h1>; 
	// Hi, Mary !
}

export default Greeting;

```
<br>

----

- #### Props (properties/ propriedades)
	- **Props** (abreviação de _properties_, ou “propriedades”) são **valores passados de um componente pai para um componente filho**. São como **parâmetros de função**, mas para **componentes React**.

```jsx
// Basic
// Component filho
import React from "react";

function Greeting(props) {
  return <p>Hi, welcome {props.name}</p>
  // Hi, welcome Jonh
}

export defualt Greeting;

// Component pai
import Greeting from "./components/Greeting"

export function App() {
	return (
		<>
			<Greeting name="Jonh" />
		</>
	) 
}

```
<br>

---

- #### Hooks
	- Funções especiais que permitem “ligar” recursos do React (como o **`useState`**, **`useEffect`**, etc.) em componentes funcionais.
	
	- **Principais recursos:**
		- `useState()` – controla estado local
		- `useEffect()` – executa efeitos colaterais (ex: requisições, timers)
		- `useContext()` – compartilha dados entre componentes sem props
		- `useRef()` – referencia elementos do DOM diretamente
<br>

- ##### State Components
	- **_`STATE`_** (ou estado) é um objeto interno de um componente que guarda valores dinâmicos, ou seja, dados que podem mudar com o tempo e causar uma re-renderização do componente. Cada vez que o **_`STATE`_** é atualizado, o **React** re-renderiza o componente para refletir o novo valor na tela utilizando o **Hook _`useState`_**

	- **_`useState`_** é um **Hook** que cria uma **ligação interna** entre o valor e o componente,  **detectando mudanças**, afim de **Re-renderiza** o componente (executando novamente a função) **atualizando o DOM virtual e o DOM real** conforme necessário.
		
```jsx
// Basic

import React, { useState } from "react";

function UsedStateComponents() {
	
	const [count, setCount] = useState(0);
	const incrementCount = () => {
		setCount((prevCount) => prevCount += 1);
	};
	
	return (
		<>
			<h1>Count: {count}</h1>
			<button onClick={incrementCount}>Increment</button>
		</>
	);
}

export default UsedStateComponents;

```
<br>
### Ciclo de vida de um componente

Os componentes passam por **três fases principais**:

1. **Montagem (Mounting)** → aparece na tela
2. **Atualização (Updating)** → re-renderiza quando props/state mudam
3. **Desmontagem (Unmounting)** → é removido da tela

Com **Hooks**, o `useEffect()` substitui os antigos métodos de ciclo de vida.
<br>

---

### **JSX**  -  mistura de **`JS`** e **`HTML`**

- Para se criar um **Component JSX REACT sem estado** e necessário a importação da biblioteca do **react**. `import React from "react"`<br>	>> **Component JSX sem estado** utilizando-se de funções (`function`)
<br>
- **Component JSX "sem estado"** _(`function`)_, por padrão pode apenas retornar "**one element**".
	_`function App() { return <h1> Hello Word </h1> }`<br>
- **Component JSX "sem estado"** _(`function`)_, para poder retornar "**any elements**" (mais de um) é, necessário estar _**envolvido dentro de outro element** como uma **div** ou um **fragmento**.
	exemplo:

	- **Div** _`return ( <div>..</div> )`_

		_`function App() {`
			`return (`
				`<div>`
				`<h1> Hello World! </h1>`
				`<p> Welcome here </p>`
				`</div>`
				`);`
		`}`_

	- **Fragment**  _`return ( <>..</> )`_
	
		_`function App() {`
			`return (`
				`<>`
				`<h1> Hello World! </h1>`
				`<p> Welcome here </p>`
				`</>`
				`);`
		`}`_
<br>

- Para se criar um **Component JSX REACT com estado** e necessário a importação da biblioteca do **react**. `import React from "react"` e a instanciação da **class Component** pertencente a biblioteca do **react** . `import React, { Component } from "react"`<br>	>> **Component JSX com estado** utilizando-se de class (`class`)
<br>
- **Components JSX com estado** _(`class`)_, 
	modo 1 (verboso)

```js

class Main extends Component {
	constructor(props) {
		super(props);
		this.state = {
			newTask: "",
		};

		this.alterInput = this.alterInput.bind(this);
	}

	alterInput(e) {
		this.setState({
			newTask: e.target.value,
		});
	}

	render() {
		const { newTask } = this.state;
		return (
			<div className="main">
				<h1>{newTask}</h1>
				<form action="#">
					<input onChange={this.alterInput} type="text" />
					<button type="submit">Send</button>
				</form>
			</div>
		);
	}
}

export default Main;

```
<br>






















































































 