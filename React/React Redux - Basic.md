
# Conceito do funcionamento de um **`reducer`**

- ### estado inicial =  { variable: value }  

- ### ação ->  escuta ação ->  manipula estado inicial -> retorna estado manipulado ou estado inicial.

---

## Instalação

```js

npm install redux react-redux

```


# PARTE 1
## Configuração básica simples ( _só para entendimento_ )

```js
// store/index.js

import { createStore } from 'redux';

const reducer = (state, action) => {
	console.log(action) // action default redux (apenas para test)
	return state;
}

const store = createStore(reducer);


// App.js 
// PROVIDER - ativar o uso do stateGlobal aos components emglobado pela TAG
import { Provider } from "react-redux";
import store from "./store";

function App() {
	return (
		<Provider store={store}>
			<h1>Hello World!</h1>
		</Provider>
	);
}

export default App;

```


> Apenas essa configuração testar o retorno de uma **`action`** redux.


---

# PARTE 2
## Criando um disparador de `action` para redux
Simulando um outro component - para utilização do **disparador**

```js
// App.js

import { Provider, useDispatch } from "react-redux";
import store from "./store";

// Função que simula um component
function OtherComponent() {
	const dispatch = useDispatch();
	
	const handleClick = () => {
		dispatch({
		type: "BUTTON_CLICKED" // BUTTON_CLICKED = IDENTIFICAÇÂO DA AÇÂO ou tipo.
	});	
	}
	
	return (
		<>
			<h1>Hello World!</h1>
			<button type="button" onClick={handleClick}>Send</button>
		</>
	);
}

function App() {	
	return (
		<Provider store={store}>
			<OtherComponent />
		</Provider>
	);
}

export default App;

```


> Regra:  **_`useDispatch`_** **não pode ser usado no mesmo componente que cria o `<Provider>`**.


---

# PARTE 3
## Manipulando o estado inicial  do redux referenciado
Retornando o **_initial state_** ou o **_state manipulado_**.

```js
// store/index.js

import { createStore } from 'redux';

// Object initialState
const initialState = {
	buttonClicked: false; // atributo
}

const reducer = (state = initialState, action) => {
	switch(action.type){
		case 'BUTTON_CLICKED': { // Identificação da action
			const newState = { ...state }; // Copia do Object (spreed Operator)
			newState.buttonClicked = !newState.buttonClicked; // alterando valor do atributo.
			return newState;
		}
	};
}

```


----

# PARTE 4
## Obtendo o **_`stateGlobal`_** da  **_`action`_**   disparada
Resultado da manipulação do estado global.

```js
