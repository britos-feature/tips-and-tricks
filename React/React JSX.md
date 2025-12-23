## **_JSX_** basic

- ##### **COMENTÁRIOS**

Comentários de um **component JSX  (_`return`_**) ,  são executados **dentro de _chaves_ { }**.

```jsx

return (
	{/* Isso é um comentário dentro do component(return) */}
);

```
<br>
Comentários de um **component JSX fora ( _`return`_ )**,  é padrão, igual a do **_JS_** 
utilizando **`//`** inline, ou **`/**/`** para bloco.

```jsx

// Isso é um comentário dentro do component

```
<br>

----

- ##### VARIÁVEIS

A declaração de varáveis normalmente são declaradas dentro do arquivo **_JSX_** porém **fora do _`return ()`_**, e para o consumo das variáveis declaradas é obrigatório a utilização de _chaves_ **{ }**

```jsx

// declaração
const var = "Jonh"

return (
	<h1> Hi, {var}!</h1>
);

```
<br>

- ##### CONDICIONAIS
	
Condicionais dentro de arquivo **_JSX_** (**_`return`_**)

#### Arquivo JSX Example

```jsx
function ExampleJSX() {

  const username = "Jonh";
  const user = { name: username, lastname: "Catter" };
  const getGreenting = (name) => `Hi, welcome ${name}`;
  const userIsLoggedIn = true;
  const userRole = "admin";

  const users = [
    { id: 1, name: "Mary"},
    { id: 2, name: "Carton"},
    { id: 3, name: "Taylor"}
  ];

  return (
    <>
      <p>Esse é meu usuário: {username}</p>
      <p>Esse é meu nome completo: {user.name} {user.lastname}</p>
      <p>{getGreenting({ username })}</p>
      <p>{ userIsLoggedIn ? <p>User logger in</p> : <p>User not logged in</p> }<p>
      {userRole === "admin" && <p>Você é um admin!</p>}
      {userRole === "admin" || <p>Você não é um admin!</p>}
    </>
  );
}
```
<br>
## Diferenças do **_JSX_** para **_HTML_**

- CLASS (style)

  - No **_HTML_** utilizamos `class` para definir classes a elementos
  - No **_JSX_** uitlizamos `className` para definir classes a elementos. (`class` é uma palavra reservada da linguagem **_JS_**, por esse motivo da mudança para `className`)
    <br>

- ATRIBUTOS

  - Em **_JSX_** os atributos dos elementos são todos utilizados com a sintaxes de **CamelCase**.<br>
    `<button onClick= {() => alert("OK, button on")}>Click here</button>` # `onClick` escrito em **CamelCase** é um atributo passado ao botão.
    <br>

- LISTAS (ul/ol -> li)
  - Em arquivos **_JSX_** é obrigatório o uso do atributo `KEY` nos elementos de lista **_HTML_**.
    - O atributo **`key`** server para distinguir cada elemento da lista e controlar atualizações na renderização.

```jsx
<>
{
  itens.map((item) => <li key={item.id}>{item.nome}</li>);
}
</>
```

<br>

> O atributo **`key`** também podem ser utilizados em elementos renderizados condicionalmente dentro de fragmentos <></> (Se você alterna entre diferentes elementos dentro de um mesmo contêiner, pode usar key para forçar o React a recriar o elemento)

<br>

```jsx
// FRAGMENTOS

<>
{
  modo === "login" ? (
    <Form key="login" tipo="login" />
  ) : (
    <Form key="cadastro" tipo="cadastro" />
  );
}
</>

// Aqui as keys ajudam o entendimento que são dois formulários diferentes, e não o mesmo reaproveitado.
```