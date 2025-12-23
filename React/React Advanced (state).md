
## State Components

#### Names:
-  **StateFull components** (com Hooks ou classes), **components COM `state`**
- **StateLess components** (utilizam apenas props), **components SEM `state`**
<br>

---

### State Components 

#### ARRAYS / OBJECTS basic
##### Alter variable  (_`state`_) - `arrays[] / objects {}` 

No React, normalmente armazenamos **arrays/ object** no **state** quando pretendemos **mudar dinamicamente** seus valores (ex: adicionar, editar, remover itens).

**Arrays e Object basics**, nos **`state`** são tratados como **Arrays e Objects comuns** como no **JS**.

**OBS:.** Para alterarmos valores do **_StateFull_ components COM `state`**,  é necessário a utilização de **`...spreed JS`** no método atualizador  do **`useState`**.  

**Exemplo:**

```jsx

import React, { useState } from "react";

// Function filho passando parâmetro de "props"
const StatesComponents = () => {

	// Object como values ao "State"
	const [users, setUsers] = useState({
		id: 1,
		name: "Jonh",
		email: "jonh@email.com",
		age: 14,
	});
	
	// Functions para alterar value do Object
	const incrementUsersAge = () => {
	
		// "SetUser()", método para alteração do "State" age
		setUsers((prevUsers) => (
			{
				...prevUsers,
				age: prevUsers.age + 1,
			}
		));
	};
	
	return (
		<>
			<h1>User.name: {users.name} Age: {users.age}</h1>
			<button onClick={incrementUserstAge}>Increment age</button>
		</>
	);
};

export default StatesComponents;

// Function pai
<StateComponents />
```
<br>
**Explicação:**

- **_`state`_** = composição que corresponde a variável de **state**
	-  `const [ users, setUsers ] = useState();`

- **_`alterUsersName()`_** = função com a lógica para alteração do value do **_`state`_**

- **_`setUsers()`_** = método altera o value do **_`state`_**, re-renderizando seu novo value a variável de **_`state`_**
<br>

---


#### ARRAYS DE OBJECTS `[{}]`

Um **Array de Objects** é simplesmente um **array (lista)** onde **cada elemento é um objeto** —  
ou seja, uma coleção de dados **estruturados**.

**Exemplo:**

```js
const users = [
  { id: 1, name: "Mary", email: "mary@email.com" },
  { id: 2, name: "John", email: "john@email.com" },
  { id: 3, name: "Cris", email: "cris@email.com" },
];
```
<br>
Como nos **Arrays e Object basics** os **array de object `[{}]`**, são tratados como **Arrays e Objects comuns** como no **JS**.

##### **Renderização basic** ( elemento individual ).
	
```jsx
return (
	<>
		<p>{users[0].id} - {users[0].name} - {users[0].email}</p>
		//       1               Mary         mary@email.com
	</>
)
```
<br>
##### Modos para **Renderização** do **array de object COMPLETO**

```jsx
import React from "react";

const StatesComponents = () => {
	const users = [
		{ id: 1, name: "Mary", email: "mary@email.com" },
		{ id: 2, name: "John", email: "john@email.com" },
		{ id: 3, name: "Cris", email: "cris@email.com" },
	];
	
	return (
		<>
			%% Utilizando " map() " %%
			{users.map((v) => (
				<p key={v.id}>
					{v.id} - {v.name} - {v.email}
				</p>
			))}
			
			
			%% Utilizando " JSON.stringify " %%
			<pre>{JSON.stringify(users, null, 2)}</pre>
			
			
			%% Utilizando " for ... of " %%
			{elements}
			
			
			%% Utilizando " reduce() " %%
			{users.reduce((acc, v) => {
				acc.push(
					<p key={v.id}>
						{v.id} - {v.name} - {v.email}
					</p>
				);
				return acc;
			}, [])}
		</>
	);
}

export default StatesComponents;

```
<br>
**Resumo:

| Método             | Retorna JSX? | Ideal para renderização | Observações                |
| ------------------ | ------------ | ----------------------- | -------------------------- |
| `map()`            | Sim          | Sim (padrão)            | Mais legível               |
| `for...of`         | Sim (manual) | Útil em casos especiais | Precisa de `push()`        |
| `reduce()`         | Sim          | Menos comum             | Mais complexo              |
| `forEach()`        | Não          | Nunca use em JSX        | Só executa                 |
| `JSON.stringify()` | Texto        | Debug                   | Mostra estrutura do objeto |
<br>
#### Alter variable  (_`state`_) - `array of object [{}]`

Diferente dos **arrays e object basics**, os **arrays de object `[{}]`** podem utilizar-se desse métodos para se **alterar o state (estado)** quando dentro de um **_componente _stateful_.** (dependo da operação).

- ##### **Adicionar** um novo objeto ao array.

```jsx

// Usando o _spread operator_ (`...`)
setUsers([...users, { id: 4, name: "Anna", email: "anna@email.com" }]);

```

> Copia todos os itens e adiciona o novo no final.
<br>
```jsx

// Add no início do array
setUsers([{ id: 4, name: "Anna", email: "anna@email.com" }, ...users]);

```

> Inseri o **`obj`** no início do array e copia o restante existente na sequencia.
<br>
- ##### **Remover** um objeto.

```jsx

// Usando o método filter()
setUsers(users.filter((user) => user.id !== 2));

```

> Remove o objeto com `id = 2`
<br>
- ##### **Editar/Atualizar** um objeto existente

```jsx

// Usando o método map()
setUsers(
  users.map((user) =>
    user.id === 2 ? { ...user, name: "John Updated" } : user
  )
);

```

> Percorre todos os objetos; altera apenas o que tiver `id = 2`.
<br>
- ##### Atualização baseada no estado anterior. (recomendado)
	- Em casos onde o novo valor depende do valor anterior (para evitar erro de sincronização)

```jsx

setUsers((prevUsers) => [
  ...prevUsers,
  { id: prevUsers.length + 1, name: "New User", email: "new@email.com" },
]);

```

> Forma **mais segura** quando o valor depende do estado atual.
<br>
- ##### Substituir o array inteiro

```jsx

setUsers([
  { id: 1, name: "Novo", email: "novo@email.com" },
  { id: 2, name: "Outro", email: "outro@email.com" },
]);

```

> Troca todo o array de uma vez.
<br>
- ##### **Limpar / Resetar** o array

```jsx

// Limpa todos os objetos (volta para array vazio).
setUsers([]);

```
<br>
- ##### **Alterar uma propriedade específica de um item**

```jsx

// mudando o e-mail do usuário com id 3
setUsers(
  users.map((u) =>
    u.id === 3 ? { ...u, email: "novo@email.com" } : u
  )
);

```
<br>
- ##### **Atualizar várias propriedades ou vários objetos**

```jsx

// Usando `map()` com condição múltipla
setUsers(
  users.map((u) =>
    u.id <= 2 ? { ...u, active: true } : { ...u, active: false }
  )
);

```
<br>
- ##### **Modificar profundamente (objetos dentro de objetos)**

```jsx

const users = [
  { id: 1, profile: { name: "Mary", age: 22 } },
];

// alterando o nome
setUsers(
  users.map((u) =>
    u.id === 1 ? { ...u, profile: { ...u.profile, name: "Maria" } } : u
  )
);

```
<br>
- ##### **Adicionar, editar e remover em sequência**

```jsx

// Combinando métodos
setUsers((prev) =>
  prev
    .filter((u) => u.id !== 2)
    .map((u) => (u.id === 1 ? { ...u, name: "Mary Edited" } : u))
    .concat({ id: 4, name: "New User", email: "new@email.com" })
);

```
<br>
- ### **RENDERIZANDO NA TELA**

```jsx

return (
  <div>
    <h1>Users</h1>
    {users.map((u) => (
      <p key={u.id}>{u.id} — {u.name} — {u.email}</p>
    ))}
  </div>
);

```


