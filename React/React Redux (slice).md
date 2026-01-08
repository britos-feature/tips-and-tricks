Dividir / separar / fatiar  **reducers**

---

# 📌 **NÃO ESQUECENDO !!**

> 	**`REDUCER`**, é um gerenciamento de **`state global`** compartilhado entre vários componentes, tornando o fluxo de dados **previsível**, **centralizado** e **fácil de depurar**.


---

# CREATE `store/index.js`

```js
import { createStore } from 'redux';

const store = createStore();

export default store;	
```
<br>

# CREATE `store/modules/rootReducer.js`
**(centralizador)**

```js
import { combineReducers } from 'redux';
import exampleReducer from './example/reducer';

export default combineReducers(
	{
		exampleReducer,
	}
);
```
<br>
# CREATE MODULES `store/modules/example/reducer.js`

> Modo de organização para que cada **`reducer`** tenha seu **module** separado.


```js
// Module example (file - reducer.js)

const initialState = {
	buttonClicked: false,
};

function reducer (state = initialState, action) {
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

export default reducer;
```
<br>
# CREATE ACTIONS  for MODULES `store/modules/example/actions.js`

> Modo de organização para que cada **`reducer`** tenha suas **actions** separadas.
	

```js
// Module example (file - actions.js)

import * as types from '../types';

export function activateButton () {
return {
		type: 'clickedButton',
	};
}
```
<br>

# CREATE TYPES GLOBAL   
(centralizado para todos os **modules**) -> **`store/modules/types.js`**

```js
// All Modules (file - types.js)

export const BUTTON_CLICKED = 'BUTTON_CLICKED';
```
<br>
# REACT-HOOK (`useDispatch`)  

> **React-hook** `useDispatch` -> disparador/ ativador  para **action - reducer** nos components desejado.

```js
// Component - pages/Login/index.js

import { useDispatch } from 'react-redux';
import { Container } from '../../styles/GlobalStyles';
import * as exampleActions from '../../store/modules/example/actions';  

function Login() {
	const dispatch = useDispatch();

	function handleClick(e) {
		e.preventDefault();
		dispatch(exampleActions.activateButton());
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
<br>
# REACT-HOOK (`useSeletor`)  

> **React-hook** `useSeletor` -> é a seleção do **`state global`** do module **reduce** da **`action`** disparada pelo **`component`** configurado.


```js
// Component /component/Header/index.js

import { Link } from 'react-router-dom';
import { FaHome, FaSignInAlt, FaUserAlt } from 'react-icons/fa';
import { useSelector } from 'react-redux';
import { Nav } from './styled';

function Header() {
	const buttonClicked = useSelector(
		(state) => state.exampleReducer.buttonClicked
	);
	
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

