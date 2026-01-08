
---

# 📦 React + Redux – Guia Completo de Funcionamento e Implantação

Este documento demonstra a arquitetura, o funcionamento e o processo de instalação de uma aplicação **React** utilizando **Redux** para gerenciamento de estado global.
**Práticas modernas (`@reduxjs/toolkit`)**.

---

## 📌 O que é Redux?

Redux é uma biblioteca de **gerenciamento de estado global** para aplicações JavaScript. Ele ajuda a controlar estados compartilhados entre vários componentes, tornando o fluxo de dados **previsível**, **centralizado** e **fácil de depurar**.

No React, o Redux é usado quando:

- Muitos componentes precisam acessar o mesmo estado
- O estado cresce e fica difícil de manter com `useState` e `props`
- É necessário histórico, rastreabilidade e previsibilidade das mudanças

---

## 🧠 Conceitos Fundamentais

### Store
A **store** é o local único onde todo o estado da aplicação fica armazenado.

### State
É o **estado global** da aplicação.

### Action
É um objeto que descreve **o que aconteceu** na aplicação.

```js
{ type: 'counter/increment' }
```

### Reducer
É uma função **pura** que recebe o estado atual e uma ação, retornando um novo estado.

### Dispatch
É o método usado para **disparar uma action**.

---

## 🏗️ Arquitetura do Projeto

Estrutura comum de pastas:

```
src/
├── app/
│   └── store.js
├── features/
│   └── example/
│       ├── exampleSlice.js
│       └── ExampleComponent.jsx
├── components/
├── services/
│   └── api.js
├── pages/
├── hooks/
├── App.jsx
└── main.jsx
```

---

## ⚙️ Funcionamento do Redux

#### 1️⃣ Store (Armazém Global)

A **store** é o objeto central que contém todo o estado global da aplicação.

#### 2️⃣ Slice (Estado + Reducers)

Um **slice** definem ( "Estado inicial", "Reducers (funções que alteram o estado" e "Actions (geradas automaticamente)")

#### 3️⃣ Fluxo de Dados no Redux

O fluxo segue sempre o mesmo padrão:


1. O usuário interage com a interface
2. Um `dispatch` é executado
3. O reducer atualiza o estado
4. Os componentes são re-renderizados

```
UI → dispatch(action) → reducer → store → UI
```

#### 4️⃣ Acessando o Estado e Actions

Hooks do React Redux,:
- "**`useSelector`**: acessa o estado
- **`useDispatch`**: dispara actions

---

## 🚀 Redux (depreciated)
 
	- Método para alteração do `state` e exibindo o `state` atual em todos os components desejados.

### 📦 Instalação

```bash

npm install redux react-redux

```


### 🗄️ Criando a Store

```js
// store/index.js

import { createStore } from 'redux';

const initialState = { 	buttonClicked: false, };

const reducer = (state = initialState, action) => {
	switch (action.type) {
		case 'BUTTON_CLICKED': {
			const newState = { ...state };
			newState.buttonClicked = !newState.buttonClicked;
			return newState;
		}
		
		default:
			return state;
	}
};

const store = createStore(reducer);
export default store;
```

## 🧩 Criando um `<Provider>`

 - O **redux**  é fornecido a aplicação devido ao **envolvimento/englobamento** dos **components** via tag **<`Provider`>** utilizando-se do atributo **`store`**

```js
// src/App.js

import { Provider } from 'react-redux';
import store from './store'; 

function App() {
	return (
	
		<Provider store={store}>
			<Router history={history}>
				<Header />
				<Routes />
				<GlobalStyles />
				<ToastContainer autoClose={3000} className="toast-container" />
			</Router>
		</Provider>
		
	);
}

export default App;
```

## 🎯 Usando Redux em Componentes 

- ### Disparar ações: `useDispatch`

```js
// src/page/Login/index.js

import { useDispatch } from 'react-redux'

function Login() {
	const dispatch = useDispatch();
		
	function handleClick(e) {
		e.preventDefault();
		dispatch({ type: 'BUTTON_CLICKED', });
	}
	
	return (
		<Container>
			<h1>Hello World</h1>
			<button type="button" onClick={handleClick}>
				Click here
			</button>
		</Container>
	);
}

export default Login;
```

- ### Ler estado: `useSelector`

```js
// src/components/Header/index.js

import { useSelector } from 'react-redux'

function Header() {
	const buttonClicked = useSelector((state) => state.buttonClicked);
	
	return (
		<Nav>
			<Link to="/"><FaHome size={24} /></Link>
			<Link to="login"><FaUserAlt size={24} /></Link>
			<Link to="123"><FaSignInAlt size={24} /></Link>
			{buttonClicked ? 'Clicked' : 'Not clicked'}
		</Nav>
	);
}

export default Header;
```
<br>

---
---


# Redux com React

## 🚀 Redux Moderno: Redux Toolkit (RTK)

Hoje, a forma recomendada de usar Redux é com **Redux Toolkit**, que:

- Reduz boilerplate
- Evita mutações acidentais
- Vem com boas práticas por padrão

Pacotes principais:

- `@reduxjs/toolkit`
- `react-redux`

---

## 📦 Instalação

### Instalar Redux

```bash
npm install @reduxjs/toolkit react-redux
```

---

## 🗄️ Criando a Store

**src/app/store.js**

```js
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

---

## 🧩 Criando um Slice

Um **slice** reúne estado, reducers e actions em um único arquivo.

**src/features/counter/counterSlice.js**

```js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
  },
})

export const { increment, decrement } = counterSlice.actions
export default counterSlice.reducer
```

> ⚠️ Parece mutação, mas o Redux Toolkit usa **Immer** por baixo dos panos.

---

## 🔌 Conectando o Redux ao React

Envolva a aplicação com o `Provider`.

**src/main.jsx**

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import { Provider } from 'react-redux'
import { store } from './app/store'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
)
```

---

## 🎯 Usando Redux em Componentes

### Ler estado: `useSelector`

### Disparar ações: `useDispatch`

**Exemplo:**

```js
import { useSelector, useDispatch } from 'react-redux'
import { increment, decrement } from './features/counter/counterSlice'

function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <p>Valor: {count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  )
}

export default Counter
```

---

## ✅ Quando Usar Redux?

Use Redux quando:

- O estado é compartilhado por muitos componentes
- A lógica de estado é complexa
- Você precisa de previsibilidade e escalabilidade

Evite Redux quando:

- Estado é simples e local
- `useState` ou `useContext` resolvem

---

## 📚 Referências

- [https://redux.js.org](https://redux.js.org)
- [https://react-redux.js.org](https://react-redux.js.org)
- [https://redux-toolkit.js.org](https://redux-toolkit.js.org)
