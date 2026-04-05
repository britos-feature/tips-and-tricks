# Styled Components – Guia de Boas Práticas

Este documento descreve como utilizar **styled-components** em projetos React de forma correta, escalável e profissional, abordando conceitos, boas práticas, armadilhas comuns e padrões recomendados.

---

## 📌 O que é styled-components?

**styled-components** é uma biblioteca de **CSS-in-JS** para React que permite escrever estilos diretamente no JavaScript, criando componentes estilizados.

Principais características:

- Estilos escopados por componente
- Suporte a props dinâmicas
- Temas globais (ThemeProvider)
- Evita conflitos de CSS
- Remove CSS não utilizado

---

## 📦 Instalação

```bash
npm install styled-components
```

(opcional – TypeScript)

```bash
npm install --save-dev @types/styled-components
```

---
---

## 🔤 Sintaxe basic 


- ### Create component (`styled.js`)

```js
// styled.js
import styled from 'styled-components';

// component (tag)
export const Title = styled.h1`
  color: blue;
`;
```
<br>

- ### Using components (`index.js`)

```js
// index.js
import { Title } from './styled';

function MyComponents() { 

  // using component (tag) 
  return <Title>My components</Title>; 
}
```


## 🔤 Sintaxe basic using \"_`props`_\" 

**Styled-components `props`** são **propriedades do React** que você passa para um componente estilizado e usa **dentro do CSS** para **alterar o estilo dinamicamente**.

👉 Em outras palavras:

> **Você usa `props` para mudar o CSS com base no estado ou intenção do componente.**


- Create components (**`styled.js`**)

```js
// styled.js
import styled from 'styled-components';

// component (tag)
export const SubTitle = styled.h1`
  color: ${(props) => (props.isRed ? red : blue)};`;
  
// component (tag) - forma moderna (destructuring)
export const SubTitle = styled.h1`
  color: ${({props}) => (props? red : blue )};`;
    
```
<br>
 > ⚠️ **Atenção**  ➜ "_**props**_" somente é uma nomenclatora, que pode ser mudada conforme necessidade.
<br>
-  Using components (**`ìndex.js`**)

```js
import { SubTitle } from './styled';

function MyComponents() {
  return (
    <>
      <SubTitle isRed>My component with 'PROPS'</SubTitle> # red (default)
      <SubTitle isRed={false}>My component with 'PROPS'</SubTitle> # blue
    </>
  );
}
```
<br>
## 🔤 Sintaxe basic using \" _`transient props`_ \"

No **styled-components**, _transient props_ são **props temporárias**, usadas **apenas para estilização**, que **não são repassadas para o DOM**.

Com transient props, você usa props **prefixadas com `$`**, que o styled-components **remove automaticamente antes de renderizar no DOM**

- Create components (**`styled.js`**)

```js
import styled from 'styled-components';

const Paragraph = styled.p`
	color: ${({ $isConfigured }) =>
		$isConfigured ? 'blue' : 'red'}; // transient props
`;

export { Paragraph };

```


-  Using components (**`ìndex.js`**)

```js
import { Paragraph } from './styled';

function Login() {
	return (
		<>
			{/* transient props "$" - para chamada da variável */}
			<Paragraph $isConfigured>
				Lorem ipsum, dolor sit amet consectetur adipisicing elit. Vel, ea!
			</Paragraph>
		</>
	);
}

export default Login;

```


---
---

## 🌍 Estilos globais

É um método pertencente a biblioteca **`styled-components`** utilizada para criação de estilos global disponibilizados para serem utilizado em todos o project. (**basta apenas importa-lo**).

Use apenas para:

- reset
- body
- fontes

```jsx
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  body {
    margin: 0;
    font-family: Arial, sans-serif;
  }
`;
```
<br>

---
---

## 🧱 Exemplo básico 

Estilização para uma `tag <button>` exemplo básico.

```jsx
// ./src/components/pages/Login/styled.js

import styled from 'styled-components';

const Button = styled.button`
  background: blue;
  color: white;
  padding: 12px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  &:hover {
    background: darkblue;
  }
`;

// Consumindo o `styled.js` components
// ./src/App.js

export function App() {
  return <Button>Clique aqui</Button>;
}
```
<br>

## 📁 Estrutura de pastas recomendada

Separar **estilo** de **lógica** melhora a manutenção:

```text
components/
 └─ pages/
     └─ Login/
         ├─ index.jsx
         └─ styles.js
```

### styles.js

```jsx
import styled from 'styled-components';

export const Button = styled.button`
  padding: 12px 16px;
  border-radius: 4px;
`;
```

### index.jsx

```jsx
import { Button } from './styles';

export function ButtonComponent({ children }) {
  return <Button>{children}</Button>;
}

// Utilização
<ButtonComponent>Click Here</ButtonComponent>;
```
<br>

---
---

## 🎨 ThemeProvider (boa prática essencial)

Evite cores e espaçamentos hard coded.

### Definindo o tema

```jsx
export const theme = {
  colors: {
    primary: '#0070f3',
    secondary: '#6b7280',
    danger: '#ef4444',
  },
  spacing: {
    sm: '8px',
    md: '16px',
    lg: '24px',
  },
};
```

### Usando no App

```jsx
import { ThemeProvider } from 'styled-components';
import { theme } from './theme';

export function App() {
  return (
    <ThemeProvider theme={theme}>
      <YourComponent />
    </ThemeProvider>
  );
}
```

### Consumindo o tema

```jsx
background: ${({ theme }) => theme.colors.primary};
```

---

## 🔁 Variantes de estilo (padrão recomendado)

Evite criar vários componentes semelhantes.

```jsx
const Button = styled.button`
  background: ${({ variant, theme }) =>
    ({
      primary: theme.colors.primary,
      secondary: theme.colors.secondary,
      danger: theme.colors.danger,
    })[variant]};

  color: white;
`;
```

Uso:

```jsx
<Button variant="primary" />
<Button variant="danger" />
```

---

## 🔧 Props dinâmicas (com moderação)

```jsx
const Box = styled.div`
  padding: ${({ padding }) => padding || '16px'};
`;
```

⚠️ Evite muitas props no CSS, pois reduz legibilidade.

---

---

## 🧠 Helpers e reutilização (mixins)

```jsx
import { css } from 'styled-components';

export const flexCenter = css`
  display: flex;
  align-items: center;
  justify-content: center;
`;
```

```jsx
const Container = styled.div`
  ${flexCenter}
`;
```

---

## ❌ Armadilhas comuns (EVITE)

### 1️⃣ Criar styled-components dentro do render

❌ ERRADO

```jsx
function App() {
  const Button = styled.button`
    color: red;
  `;
  return <Button />;
}
```

✅ CORRETO

```jsx
const Button = styled.button`
  color: red;
`;

function App() {
  return <Button />;
}
```

---

### 2️⃣ CSS gigante em um único componente

❌

```jsx
const Card = styled.div`
  /* dezenas ou centenas de linhas */
`;
```

✅

```jsx
const CardHeader = styled.div`...`;
const CardBody = styled.div`...`;
```

---

### 3️⃣ Abusar de !important

❌

```css
color: red !important;
```

styled-components já resolve escopo de estilos.

---

### 4️⃣ Usar lógica JS pesada no CSS

❌

```jsx
background: ${({ theme }) => complexFunction(theme)};
```

Prefira lógica simples e previsível.

---

### 5️⃣ Usar styled-components como inline style

❌

```jsx
<Button style={{ marginTop: 10 }} />
```

✅

```jsx
const Button = styled.button`
  margin-top: ${({ mt }) => mt}px;
`;
```

---

## 🧪 TypeScript (boa prática)

Sempre tipar props:

```tsx
type ButtonProps = {
  variant: "primary" | "secondary" | "danger";
};

const Button = styled.button<ButtonProps>`
  background: ${({ variant }) => ...};
`;
```

---
---

# Transient Props

No **styled-components**, _transient props_ são **props temporárias**, usadas **apenas para estilização**, que **não são repassadas para o DOM**.

Elas são identificadas por um **prefixo `$`** no nome da prop.

- ## **Antes dos transient props, era comum fazer algo assim:**

```js
const Button = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
`;

// consumindo
<Button primary>Salvar</Button>
```
<br>

❌ **Problema:**
A prop **`primary`** acabava indo parar no HTML final.

```html
<button primary>Salvar</button>
```

Isso causa:

- Warnings no console do React
- HTML inválido
- Problemas de acessibilidade e manutenção


- ## Depois dos Transient Props (`$`) - "recomendado"
	- Com transient props, você usa props **prefixadas com `$`**, que o styled-components **remove automaticamente antes de renderizar no DOM**.


```js
const Button = styled.button`
  background: ${({ $primary }) => ($primary ? 'blue' : 'gray')};
  color: white;
`;

// consumindo
<Button $primary>Salvar</Button>
<Button>Cancelar</Button>
```


✅ Correto:
HTML gerado limpo

```html
<button>Salvar</button>
<button>Cancelar</button>
```

---
---

## ⚡ Performance e SSR

- Não crie styled-components dinamicamente
- Em **Next.js**, configure SSR corretamente para evitar FOUC
- Evite recriações desnecessárias de componentes

---

## ✅ Resumo de boas práticas

✔ Use ThemeProvider
✔ Separe estilos da lógica
✔ Prefira variants
✔ Componentes pequenos
✔ CSS simples e previsível

---

## 🚫 O que evitar

❌ CSS enorme
❌ !important
❌ Lógica complexa no CSS
❌ Styled-components dentro do render

---
