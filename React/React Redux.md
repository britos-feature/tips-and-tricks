
---

# 📦 React + Redux – Guia Completo de Funcionamento e Implantação

Este projeto demonstra a arquitetura, o funcionamento e o processo de implantação de uma aplicação **React** utilizando **Redux** para gerenciamento de estado global.

---

## 🧠 O que é React + Redux?

- **React**: biblioteca JavaScript para construção de interfaces de usuário baseadas em componentes.
- **Redux**: biblioteca para gerenciamento de estado global previsível.
- **Redux Toolkit (RTK)**: conjunto oficial de ferramentas que simplifica o uso do Redux.

O **Redux** é especialmente útil quando:

- Muitos componentes precisam compartilhar o mesmo estado
- O estado da aplicação é complexo
- É necessário controle e previsibilidade das mudanças de estado
   
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

## 📦 Instalação (basic)

```bash

npm install redux react-redux

```


---


## ⚙️ Funcionamento do Redux

### 1️⃣ Store (Armazém Global)

A **store** é o objeto central que contém todo o estado global da aplicação.

 - ### Basic

```js
import { createStore } from 'redux';

const reducer = (state, action) => {
	switch(action.type) {
		case 'BUTTON_CLICKED':
			return state;
		
		default:
			return state;
	}
};

const store = createStore(reducer);

export default store;
```


>  O **redux**  é fornecido à aplicação/ components - **envolvendo/englobando** os mesmos pela tag **<`Provider`>** passando como atributo o **`store`**

```jsx
import { Provider } from 'react-redux';
import store from './store';

<Provider store={store}>
   <App />
</Provider>
```


> Escutando a `action` **redux in components**

```js
import { useDispatch } from 'react-redux'

const dispatch = useDispatch();

const handleClick = () => dispatch({ type: 'BUTTON_CLICKED' });

function APP () {
	return <button type='button' onClick={handleClick}>Click here</button>
}

export default App;
```


---


```js
import { configureStore } from '@reduxjs/toolkit';
import exampleReducer from '../features/example/exampleSlice';

export const store = configureStore({
  reducer: {
    example: exampleReducer,
  },
});
```

Ela é fornecida à aplicação usando o `Provider`:

```jsx
import { Provider } from 'react-redux';
import { store } from './app/store';

<Provider store={store}>
   <App />
</Provider>
```

---

### 2️⃣ Slice (Estado + Reducers)

Um **slice** define:

- Estado inicial
    
- Reducers (funções que alteram o estado)
    
- Actions (geradas automaticamente)
    

```js
import { createSlice } from '@reduxjs/toolkit';

const exampleSlice = createSlice({
  name: 'example',
  initialState: {
    value: 0,
  },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
  },
});

export const { increment } = exampleSlice.actions;
export default exampleSlice.reducer;
```

---

### 3️⃣ Fluxo de Dados no Redux

O fluxo segue sempre o mesmo padrão:

```
UI → dispatch(action) → reducer → store → UI
```

1. O usuário interage com a interface
    
2. Um `dispatch` é executado
    
3. O reducer atualiza o estado
    
4. Os componentes são re-renderizados
    

---

### 4️⃣ Acessando o Estado e Actions

Hooks do React Redux:

- `useSelector`: acessa o estado
    
- `useDispatch`: dispara actions
    

```jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment } from './exampleSlice';

const ExampleComponent = () => {
  const value = useSelector((state) => state.example.value);
  const dispatch = useDispatch();

  return (
    <>
      <p>Valor: {value}</p>
      <button onClick={() => dispatch(increment())}>
        Incrementar
      </button>
    </>
  );
};
```

---

## 🌐 Redux com Requisições Assíncronas

Usando `createAsyncThunk`:

```js
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchData = createAsyncThunk(
  'example/fetchData',
  async () => {
    const response = await fetch('https://api.exemplo.com/data');
    return response.json();
  }
);
```

Estados comuns:

- `pending`
    
- `fulfilled`
    
- `rejected`
    

Isso facilita loading e tratamento de erros.

---

## 🚀 Implantação (Deploy)

### 🔧 Build de Produção

```bash
npm run build
```

Gera a pasta:

```
dist/   (Vite)
build/  (Create React App)
```

---

## ☁️ Deploy com Vercel

1. Suba o projeto para o GitHub
    
2. Acesse [https://vercel.com](https://vercel.com)
    
3. Importe o repositório
    
4. Configure:
    
    - Framework: React / Vite
        
    - Build Command: `npm run build`
        
    - Output Directory: `dist`
        
5. Deploy automático 🎉
    

---

## ☁️ Deploy com Netlify

1. Acesse [https://netlify.com](https://netlify.com)
    
2. Novo site → Import from Git
    
3. Configure:
    
    - Build command: `npm run build`
        
    - Publish directory: `dist`
        
4. Deploy concluído
    

---

## 🐳 Deploy com Docker (Opcional)

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["npx", "serve", "dist"]
```

```bash
docker build -t react-redux-app .
docker run -p 3000:3000 react-redux-app
```

---

## 🔐 Variáveis de Ambiente

Exemplo `.env`:

```
VITE_API_URL=https://api.exemplo.com
```

Uso:

```js
import.meta.env.VITE_API_URL
```

---

## ✅ Boas Práticas

- Use Redux apenas para **estado global**
- Prefira Redux Toolkit
- Organize por **features**
- Evite lógica pesada nos componentes
- Utilize middlewares para efeitos colaterais

---

## 📚 Tecnologias Utilizadas

- React
- Redux Toolkit
- React Redux
- Vite / CRA
- JavaScript / TypeScript

---

