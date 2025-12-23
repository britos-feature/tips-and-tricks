

# Class react (explicação) 
Método **legado**, não mais utilizado atualmente.

```jsx
import React from "react";

class Main extends React.Component {

	render(
		return(
			<>
				<h1>Hello World, React!</h1>
				<p>Welcome ...</p>
			</>
		);
	);
}

export default Main;

```
<br>

- **_`Class`_**
	Define um **componente de classe** no **REACT**, ou uma **Função. ( forma “antiga”** antes dos _Hooks_) de criar componentes React.

- **_Comportamento_** <span style="color: red"><b>(Importante!)</b></span>  
    Todo componente de classe **precisa estender (`extends`)** a classe **_`React.Component`_** para ter acesso às funções internas do React, como **`render()`**, **`state`** e **`props`**.

- **_render()_**
	Método obrigatório em um componente de classe React, um **Função** que retorna **o que será renderizado na tela (UI)** — geralmente elementos JSX.
		**Importante:**
		Sempre deve **retornar um único elemento JSX**, que pode ser um `div`, ou um fragmento (`<> </>`).

- **_return()_**
	É a parte da **função* _`render()`_** que **define o conteúdo visual** do componente, ou seja, é o retorno da **Função**, onde tudo que estiver dentro desse **_`return`_** será renderizado no navegador e convertido em HTML real.

- **_<> \</> (fragments)_**
	Um **Fragmento React** (`React.Fragment` abreviado), serve para **agrupar múltiplos elementos JSX** sem adicionar uma `div` extra no HTML final.
	
- **_export default_**
	- Exporta o componente para que possa ser importado em outro arquivo.


## Class React basic with: (state, props, etc ...)

```jsx
import React from "react";

class Main extends React.Component {
	state = {
		mensagem: "Hello world, React!",
		counter: 0
	};	

	handleClick = () => {
		this.setState({ counter: this.state.counter + 1 });
	};
	
	render(
		
		const { mensage, counter } = this.state;
		const { name } = this.props;
		
		return(
			<>
				<h1>{mensage}</h1>
				<h2>Welcome, {name}!</h2>
				<p>You clicked {counter} times</p>
				<button onClick={this.handleClick}>click here</button>
			</>
			
		);
	);
}

export default Main;

```
<br>
### Consumindo component Class basic with (state, props)

```jsx
import React from "react";
import Main from "./Main";

function App() {
  return (
    <div>
      <Main name="Britos" />
    </div>
  );
}

export default App;

```
<br>
### Resultado visual no navegador

```css
Olá, React com classe!
Bem-vindo, Britos!
Você clicou 0 vezes.
[ Clique aqui ]
```

> Cada clique no botão aumenta o contador **(counter)**


---
---

# Função react (explicação)
**Função  react**, é simplesmente uma **função JavaScript** que **retorna JSX** (HTML dentro do JavaScript) e representa **parte da interface (UI)** da aplicação React.

```jsx
function Main () {
	return <h1>Hello World!</h1>;
}

export default Main;
```
<br>

- **function**
	É uma função normal do JavaScript ( **`function Main()`** )

- **return()**
	Retorna um elemento **JSX** ( **`<h1>Olá, mundo!</h1>`** )

-  Pode ser usado em outro lugar assim: **`<Main />`** _(após ser importado)_
<br>

## Função react with (state, props, etc...) `Hooks` 
Os **Hooks** foram introduzidos no **React 16.8 (2019)**.
Os **Hooks** são funções especiais do React para adicionar comportamento dinâmico,  eles permitem usar **recursos avançados** — como **estado (state)** e **ciclo de vida (life cycle)** — **dentro de componentes funcionais**, sem precisar usar **_`class`_**.

_Antes dos Hooks:_
	 Só componentes de **_`class`_** tinham **_`state`_** e **_`componentDidMount`_**, etc.

_Depois dos Hooks:_
	 Componentes **funcionais** também podem ter **state, effect** e **lógica complexa**.


### Função react basic with( Hooks )
 
```jsx
import React, { useState } from "react";

function Main() {
  const [count, setCount] = useState(0);

  // Função que altera o estado
  function handleClick() {
    setCount(count + 1);
  }

  return (
    <>
      <h1>Contador: {count}</h1>
      <button onClick={handleClick}>Incrementar</button>
    </>
  );
}

export default Counter;

```
