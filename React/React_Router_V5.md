# Guia Completo do React Router DOM v5

Este guia foi criado para servir como **referência completa e prática** do **React Router DOM v5**, muito utilizado em projetos React legados e ainda presente em muitos sistemas em produção.

> ⚠️ Observação: Este guia é **exclusivo para a versão 5**. A v6 possui diferenças grandes de API e conceitos.

---

- ## 📌 O que é o React Router DOM?

O **React Router DOM** é uma biblioteca que permite criar **rotas e navegação** em aplicações React do tipo **SPA (Single Page Application)**.

Ele controla qual componente será renderizado com base na **URL atual**, sem recarregar a página.

---

- ## 📦 Instalação (v5)

```bash
npm install react-router-dom@5
# ou
yarn add react-router-dom@5
```

---

- ## 🧠 Conceitos Fundamentais

### SPA (Single Page Application)

- Apenas um `index.html`
- Navegação sem reload
- React controla o DOM

### Roteamento

- URL muda → componente muda
- Histórico controlado pelo navegador

---

## 🚀 Configuração Básica

### BrowserRouter

Componente raiz responsável por habilitar o roteamento.

```jsx
import { BrowserRouter } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes />
    </BrowserRouter>
  );
}
```

---

- ## 🛣️ Criando Rotas

### Route

Define qual componente será renderizado para uma rota.

```jsx
import { Route } from 'react-router-dom';

<Route path="/about" component={About} />;
```

### Switch

Renderiza **apenas a primeira rota compatível**.

```jsx
import { Switch, Route } from 'react-router-dom';

<Switch>
  <Route path="/" exact component={Home} />
  <Route path="/about" component={About} />
</Switch>;
```

### exact

Evita correspondência parcial.

```jsx
<Route path="/" exact component={Home} />
```

---

- ## 🔗 Navegação

### Link

Substitui a tag `<a>`.

```jsx
import { Link } from 'react-router-dom';

<Link to="/about">Sobre</Link>;
```

### NavLink

Permite aplicar estilo quando ativo.

```jsx
<NavLink to="/dashboard" activeClassName="active">
  Dashboard
</NavLink>
```

---

- ## 🧩 Rotas Dinâmicas

### Parâmetros de rota

```jsx
<Route path="/user/:id" component={User} />
```

### Acessando parâmetros

```jsx
function User({ match }) {
  return <h1>ID: {match.params.id}</h1>;
}
```

---

- ## 🧭 Hooks do React Router v5

### useHistory

Manipula o histórico.

```jsx
import { useHistory } from 'react-router-dom';

const history = useHistory();
history.push('/login');
```

### useLocation

Acessa informações da URL atual.

```jsx
const location = useLocation();
console.log(location.pathname);
```

### useParams

Obtém parâmetros da rota.

```jsx
const { id } = useParams();
```

### useRouteMatch

Verifica se uma rota corresponde.

```jsx
const match = useRouteMatch('/admin');
```

---

- ## 🔒 Rotas Protegidas (Private Routes)

```jsx
function PrivateRoute({ component: Component, ...rest }) {
  return (
    <Route
      {...rest}
      render={(props) =>
        isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
      }
    />
  );
}
```

---

- ## 🔁 Redirect

```jsx
import { Redirect } from 'react-router-dom';

<Redirect to="/login" />;
```

---

- ## 🧱 Rotas Aninhadas

```jsx
<Route path="/dashboard" component={Dashboard} />
```

Dentro do componente:

```jsx
<Route path="/dashboard/profile" component={Profile} />
```

---

- ## ⚙️ render vs component

### component

- Cria o componente automaticamente

```jsx
<Route component={Home} />
```

### render

- Permite lógica extra

```jsx
<Route render={() => <Home />} />
```

---

- ## 🧪 404 – Rota Não Encontrada

```jsx
<Route component={NotFound} />
```

Sempre deixe **por último no Switch**.

---

- ## 🗂️ Organização de Pastas (Sugestão)

```
src/
 ├─ pages/
 ├─ routes/
 │   └─ index.js
 ├─ components/
 └─ App.js
```

---

- ## 🆚 Diferenças Importantes para a v6

| v5        | v6       |
| --------- | -------- |
| Switch    | Routes   |
| component | element  |
| exact     | padrão   |
| Redirect  | Navigate |

---

- ## ✅ Boas Práticas

* Use `Switch`
* Centralize as rotas
* Evite lógica pesada dentro de `Route`
* Prefira `NavLink` para menus

---

- ## 📚 Quando usar React Router v5 hoje?

* Projetos legados
* Sistemas grandes em produção
* Manutenção de código antigo

Para novos projetos, **prefira a v6**.
