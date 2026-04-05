
# styled-components — Conceitos Avançados

## 1️⃣ Transient Props (recap rápido)

Você já viu isso, mas é **fundamental no nível avançado**.

```js

const Button = styled.button`
   background: ${({ $variant }) => 
	   $variant === 'primary' ? 'blue' : 'gray'}; `;

```

✔ Evita poluir o DOM  
✔ Padrão moderno e recomendado

---

## 2️⃣ `css` helper (composição real de estilos)

O `css` serve para **reutilizar blocos de estilo**, não apenas variáveis.

```js

import styled, { css } from 'styled-components';

const flexCenter = css`
   display: flex;
   align-items: center;
   justify-content: center;
`;

const Box = styled.div`
   ${flexCenter};
   height: 100px; 
`;
```

### Com props

```js
const buttonVariants = {
   primary: css`
	   background: blue;
	   color: white;
  `,
  
   danger: css`
       background: red;
       color: white;
   `,
};

const Button = styled.button`
	${({ $variant }) => buttonVariants[$variant]} 
`;
```

🧠 **Isso escala MUITO melhor que ternários gigantes**

---

## 3️⃣ Styled-components polimórfico (`as`)

Permite trocar o elemento HTML **sem duplicar estilos**.

```js

const Text = styled.p`
  font-size: 16px; 
`;

// consumindo  
<Text as="span">Inline</Text> 
<Text as="h1">Título</Text>

```

### Componente customizado

```js

`<Text as={Link} to="/home" />`

```

---

## 4️⃣ `attrs` — props derivadas e defaults

Usado para:

- Valores padrão
- Props computadas
- Integração com libs externas

```js

const Input = styled.input.attrs(props => ({   type: props.type || 'text', }))`
	border: 1px solid #ccc; 
`;

```

### Com transient props

```js

const Input = styled.input.attrs(({ $invalid }) => 
	({ 'aria-invalid': $invalid, }))`   
		border-color: ${({ $invalid }) => ($invalid ? 'red' : '#ccc')}; 
	`;

```


---

## 5️⃣ Theming avançado (`ThemeProvider`)

### Estrutura profissional de tema

```js

const theme = {
	colors: {
		primary: '#0984e3',     
		danger: '#d63031',   
	},   
	
	spacing: {
	    sm: '8px',     
	    md: '16px',   
	},
	
    breakpoints: {
        md: '768px',
    },
};

```


Uso:

```js

const Box = styled.div`
	padding: ${({ theme }) => theme.spacing.md};
	color: ${({ theme }) => theme.colors.primary}; 
`;

```

### Media queries via tema

```js

const Container = styled.div`   
	width: 100%;    
	
	@media (min-width: ${({ theme }) => theme.breakpoints.md}) {
		width: 768px;   
	}
`;

```

---

## 6️⃣ Estilos condicionais complexos (sem bagunça)

❌ Evite:

```js

background: ${p => p.a ? 'red' : p.b ? 'blue' : 'green'};

```

✅ Prefira:

```js

const variants = {   
	success: 'green',   
	warning: 'orange',   
	error: 'red', 
};  

// Consumindo
background: ${({ $status }) => variants[$status]};

```

---

## 7️⃣ Estendendo estilos (`styled(Component)`)

```js

const BaseButton = styled.button`   
	padding: 8px 16px; 
`;

const DangerButton = styled(BaseButton)`
	background: red; 
`;

```

⚠ **Cuidado**: herança excessiva pode criar acoplamento forte.

---

## 8️⃣ `shouldForwardProp` (controle fino do DOM)

Alternativa avançada aos transient props.

```js

const Button = styled('button', {
	shouldForwardProp: prop => prop !== 'variant', })`
		background: ${({ variant }) => variant === 'primary' ? 'blue' : 'gray'}; `;
		
```

📌 Use quando:

- Está criando **design system**
- Precisa filtrar props dinamicamente

---

## 9️⃣ Performance (muito importante)

### ❌ Anti-padrão

```js

const Button = styled.button`   
	background: ${() => Math.random() > 0.5 ? 'red' : 'blue'}; 
`;

```

Isso gera **novas classes toda renderização**.

### ✅ Boa prática

- Evite funções não determinísticas
- Prefira **mapas de variantes**
- Componentes estilizados **fora do render**

---

## 🔟 Testes com styled-components

Você pode testar estilos:

```js

expect(button).toHaveStyle(`
	background: blue; 
`);

```

Ou snapshot:

```js

expect(container.firstChild).toMatchSnapshot();

```


---

## Arquitetura recomendada 🏗️

`components/   Button/     styles.ts     Button.tsx     types.ts theme/   colors.ts   spacing.ts`

Separar **estilo ≠ lógica** melhora manutenção.

---

## Resumo de nível sênior 🧠

✔ Use **transient props**  
✔ Prefira **variants via `css`**  
✔ Theme bem tipado  
✔ Evite herança profunda  
✔ Controle o que vai para o DOM  
✔ Pense em performance desde o início
