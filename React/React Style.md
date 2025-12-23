No **React** pode ser utilizar **_`style`_** de várias formas, desde o uso de **CSS tradicional** até **CSS-in-JS**
Abaixo algumas abordagem:

---

- ## CSS tradicional (arquivo `.css`)
	-  A forma mais comum e simples.
#### Exemplo:

```jsx
// App.js
import React from "react";
import "./App.css"; // importa o arquivo CSS

function App() {
  return <h1 className="titulo">Olá React!</h1>;
}

export default App;
```

```css
/* App.css */
.titulo {
  color: blue;
  text-align: center;
  font-size: 2rem;
}
```

> **Vantagem:** simples e direto  
   **Desvantagem:** classes globais podem gerar conflito de nomes

---

WE
- ## CSS Modules (`.module.css`)
	-  Isola o escopo dos estilos (evita conflitos entre componentes).
#### Exemplo:

```jsx
// Button.jsx
import React from "react";
import styles from "./Button.module.css"; // Importa como objeto

function Button() {
  return <button className={styles.btn}>Clique aqui</button>;
}

export default Button;
```

```css
/* Button.module.css */
.btn {
  background-color: purple;
  color: white;
  padding: 10px 20px;
  border-radius: 8px;
}
```

> **Vantagem:** cada estilo é exclusivo ao componente  
   **Desvantagem:** não permite facilmente temas globais

---

- ## Inline Styles (estilo direto no JSX)
	- Define estilos como **objeto JavaScript**.
### Exemplo:

```jsx
function Card() {
  const cardStyle = {
    backgroundColor: "#eee",
    padding: "20px",
    borderRadius: "10px",
  };

  return <div style={cardStyle}>Meu Card</div>;
}

export default Card;
```

> **Vantagem:** simples e dinâmico (pode usar variáveis JS)  
   **Desvantagem:** não suporta pseudo-classes (`:hover`, `:focus` etc.)

---

- ## Styled Components (biblioteca externa)
	-  Usa **CSS-in-JS**. É muito popular.
#### Instalação:

```bash
npm install styled-components
```

### Exemplo:

```jsx
import styled from "styled-components";

const Button = styled.button`
  background: #3498db;
  color: white;
  padding: 10px 20px;
  border-radius: 8px;
  border: none;

  &:hover {
    background: #2980b9;
  }
`;

function App() {
  return <Button>Clique</Button>;
}

export default App;
```

> **Vantagem:** estilos dinâmicos e poderosos  
   **Desvantagem:** adiciona dependência extra

---

## 🧩 5. Frameworks de estilo (Tailwind, MUI, etc.)

### Exemplo com **Tailwind CSS**:

```bash
npm install -D tailwindcss
npx tailwindcss init
```

```jsx
function App() {
  return (
    <button className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">
      Clique
    </button>
  );
}
```

>  **Vantagem:** rápido, produtivo e responsivo
>  **Desvantagem:** código JSX pode ficar “poluído” com muitas classes

---

#### Resumo rápido:

| Método            | Isolamento | Dinamismo | Dependência externa | Ideal para         |
| ----------------- | ---------- | --------- | ------------------- | ------------------ |
| CSS comum         | ❌          | ❌         | ❌                   | Projetos simples   |
| CSS Modules       | ✅          | ⚙️        | ❌                   | Apps médios        |
| Inline style      | ✅          | ✅         | ❌                   | Estilos dinâmicos  |
| Styled Components | ✅          | ✅         | ✅                   | Apps modernos      |
| Tailwind/MUI      | ✅          | ⚙️        | ✅                   | Produtividade alta |
