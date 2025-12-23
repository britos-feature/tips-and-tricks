O **_`useEffect`_** é mais um **_hooks_** do React como o **_`useState`_**. Um método dentro de um component **REACT**. O **_`useEffect`_**, permite **execute efeitos colaterais** em componentes funcionais, ou seja, ações que acontecem _fora_ do fluxo normal de renderização do React.

## “Efeito colateral”

São ações que o componente realiza além de apenas renderizar a interface, como:

- Buscar dados de uma API (`fetch`, `axios`, etc.);  
- Manipular o DOM manualmente;
- Registrar ou limpar _event listeners_;
- Usar _timers_ (`setTimeout`, `setInterval`);
- Atualizar o título da página, etc.


#### **Sintaxe básica**

```jsx

import React, { useEffect } from "react";

function UsedHookEfect() {

  useEffect(() => {
    console.log("O componente foi renderizado!");
  });

  return <h1>Olá, React!</h1>;
}

export default UsedHookEfect;
```

> declarado da forma acima, o **_`useEffect`_** é executado **toda vez que o componente renderiza** (inclusive nas atualizações).
<br>
---

## Modos de utilização **Hook Effect**

 O _**`useEffect`**_ aceita **dois argumentos** (blocos de comando e array de dependências)

```jsx

useEffect(() => {
	// bloco de comandos...
}, [dependências]);

```

> O **segundo parâmetro** (o array de dependências `[]`) define **quando** o efeito será executado.
<br>
- ### **Sem array → roda em toda renderização**

```jsx

useEffect(() => {
  console.log("Renderizou!");
});

```
<br>
- ### **Array vazio `[]` → roda apenas uma vez (quando o componente é montado)**

```jsx

useEffect(() => {
  console.log("Montado uma vez!");
}, []);

```
<br>
- ### **Com dependências → roda sempre que uma delas mudar**
	- Utilizado juntamente com o **_Hook `useState`_**

```jsx

const [count, setCount] = useState(0);

useEffect(() => {
  console.log("O count mudou:", count);
}, [count]);

```


---

## Limpando efeitos (cleanup)

O **_`useEffect`_** pode retornar uma função para **limpar** o efeito anterior — útil para:

- cancelar requisições;
- limpar _event listeners_;
- parar timers.

```jsx

useEffect(() => {
  const timer = setInterval(() => {
    console.log("Executando...");
  }, 1000);

  return () => clearInterval(timer); // executa ao desmontar o componente
}, []);

```


---

## **Exemplo prático (buscando dados de uma API)**

```jsx

import React, { useState, useEffect } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function fetchData() {
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      const data = await response.json();
      setUsers(data);
    }

    fetchData();
  }, []); // executa apenas uma vez

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

```
<br>
#### Resumo rápido:

| Situação               | Sintaxe                                         | Quando executa               |
| ---------------------- | ----------------------------------------------- | ---------------------------- |
| Sempre                 | `useEffect(fn)`                                 | Em toda renderização         |
| Uma vez (montagem)     | `useEffect(fn, [])`                             | Apenas ao montar             |
| Dependência específica | `useEffect(fn, [var])`                          | Quando `var` muda            |
| Com limpeza            | `useEffect(() => { ...; return cleanup; }, [])` | Executa limpeza ao desmontar |

---

## **Exemplo completo: fetch de dados com estado e limpeza**

```jsx

import React, { useState, useEffect } from "react";

function UsersList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let isMounted = true; // controle manual (boa prática)

    async function fetchUsers() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const data = await response.json();

        if (isMounted) {
          setUsers(data);
          setLoading(false);
        }
      } catch (error) {
        console.error("Erro ao buscar usuários:", error);
      }
    }

    fetchUsers();

    // Cleanup: define que o componente saiu da tela
    return () => {
      isMounted = false;
    };
  }, []);

  if (loading) return <p>Carregando...</p>;

  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}

```
<br>
