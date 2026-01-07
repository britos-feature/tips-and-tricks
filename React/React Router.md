
---

# 📘 React Router — Guia Completo (v5 vs v6)

Este documento tem como objetivo explicar de forma **clara, prática e completa** o funcionamento do **React Router**, com foco na **versão 6+**, incluindo:

- Conceitos fundamentais
- Diferenças entre **React Router v5 e v6**
- Exemplos de código
- Boas práticas
- Armadilhas comuns
- Estrutura recomendada de projeto

---

## 📌 O que é o React Router?

O **React Router** é a biblioteca padrão para **gerenciamento de rotas** em aplicações React.
Ele permite criar **Single Page Applications (SPA)**, onde:

- A página **não recarrega**
- A URL muda dinamicamente
- O React decide qual componente renderizar

---

## 📦 Instalação

```bash
npm install react-router-dom
```

> Atualmente, utilize sempre **react-router-dom v6+**

---

## 🧠 Conceitos Fundamentais

- **Browser Router** → provê o contexto de rotas
- **Routes** → container das rotas
- **Route** → define o caminho e o componente
- **Link** → navegação sem reload
- **Hooks** → `useNavigate`, `useParams`, `useLocation`
- **Outlet** → renderização de rotas filhas

---

## 🚦 Estrutura Básica (v6+)

- ### Envolvendo a aplicação

```jsx
import { BrowserRouter } from 'react-router-dom';
import AppRoutes from './routes';

function App() {
  return (
    <BrowserRouter>
      <AppRoutes />
    </BrowserRouter>
  );
}

export default App;
```

---

- ### Definindo rotas

```jsx
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Login from './pages/Login';

function AppRoutes() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
    </Routes>
  );
}

export default AppRoutes;
```

📌 **Na v6**:

- ❌ `component={Home}` 
- ✅ `element={<Home />}`
- ❌ `Switch`
- ✅ `Routes`
- ❌ `exact`

---

## 🔗 Navegação entre páginas

- ### Usando `Link` (recomendado)

```jsx
import { Link } from 'react-router-dom';

<Link to="/login">Ir para Login</Link>
```

❌ Evite `<a href="">` **(recarrega a página)**

---

### Navegação programática (`useNavigate`)

```jsx
import { useNavigate } from 'react-router-dom';

function Login() {
  const navigate = useNavigate();

  function handleLogin() {
    navigate('/dashboard');
  }

  return <button onClick={handleLogin}>Entrar</button>;
}
```

Extras:

```js
navigate(-1); // voltar
navigate('/login', { replace: true });
```

---

## 🧩 Rotas Dinâmicas (Parâmetros)

Rota:

```jsx
<Route path="/users/:id" element={<User />} />
```

Componente:

```jsx
import { useParams } from 'react-router-dom';

function User() {
  const { id } = useParams();
  return <h1>Usuário {id}</h1>;
}
```

---

## 🧱 Rotas Aninhadas (Layouts)

### Definição das rotas

```jsx
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route index element={<DashboardHome />} />
  <Route path="profile" element={<Profile />} />
</Route>
```

### Layout

```jsx
import { Outlet } from 'react-router-dom';

function DashboardLayout() {
  return (
    <>
      <Header />
      <Sidebar />
      <Outlet />
    </>
  );
}
```

✔ Ideal para menus, headers e sidebars  
✔ Organização muito superior à v5

---

## 🔒 Rotas Protegidas (Autenticação)

```jsx
import { Navigate } from 'react-router-dom';

function PrivateRoute({ children }) {
  const isAuthenticated = true; // exemplo

  return isAuthenticated
    ? children
    : <Navigate to="/login" replace />;
}
```

Uso:

```jsx
<Route
  path="/dashboard"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
/>
```

---

## ❌ Página 404 (Not Found)

```jsx
<Route path="*" element={<NotFound />} />
```

---

## 🔄 React Router v5 vs v6 — Diferenças

### Principais mudanças

|Conceito|v5|v6|
|---|---|---|
|Switch|✅|❌|
|Routes|❌|✅|
|exact|✅|❌|
|component|✅|❌|
|element|❌|✅|
|useHistory|✅|❌|
|useNavigate|❌|✅|
|Redirect|✅|❌|
|Navigate|❌|✅|
|withRouter|✅|❌|
|Ordem das rotas|Importa|Não importa|

---

### Exemplo de mudança (navegação)

#### v5

```js
history.push('/dashboard');
```

#### v6

```js
navigate('/dashboard');
```

---

## ⚙️ Nova API (v6.4+)

Introdução de uma abordagem mais moderna:

- `createBrowserRouter`
- `loader`
- `action`
- `errorElement`

Exemplo:

```jsx
const router = createBrowserRouter([
  {
    path: '/',
    element: <Layout />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
    ],
  },
]);
```

➡ Aproxima o React Router de frameworks como **Remix** e **Next.js**

---

## ⚠️ Armadilhas Comuns

❌ Usar `Switch`  
❌ Usar `component=`  
❌ Usar `<a href>`  
❌ Esquecer o `<Outlet />`  
❌ Colocar lógica de autenticação dentro da rota  
❌ Misturar layout com lógica de negócio

---

## ✅ Boas Práticas

✔ Criar um arquivo exclusivo para rotas  
✔ Separar **layouts**, **páginas** e **componentes**  
✔ Usar **lazy loading** (`React.lazy`)  
✔ Centralizar regras de autenticação  
✔ Manter rotas simples e previsíveis

Exemplo com lazy loading:

```jsx
const Login = React.lazy(() => import('./pages/Login'));
```

---

## 📁 Estrutura de Projeto Recomendada

```txt
src/
├── pages/
│   ├── Home/
│   ├── Login/
│   └── Dashboard/
├── layouts/
│   └── DashboardLayout.jsx
├── routes/
│   └── index.jsx
├── components/
└── App.jsx
```

---

## 🧭 Qual versão usar?

✅ **Sempre React Router v6+**  
❌ v5 está obsoleta (deprecated)

---

## 📚 Referência

- Documentação oficial: [https://reactrouter.com](https://reactrouter.com)