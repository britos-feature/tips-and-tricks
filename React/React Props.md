## PROPS (propriedades)

A palavra **`props`** vem de “properties” (propriedades).
**`PROPS`** são valores passados de um componente pai para um componente filho, assim como parâmetros de uma função em JavaScript.

**Em resumo:**
🔹 Props servem para enviar dados para dentro de componentes.

**Entendimento**:
Pense num componente como uma função comum, recebendo parâmetro.

```js
// function basic JS

function greeting(name) {
  return <h1>Hi, welcome {name}!</h1>;
}

greeting("Mary"); // Hi, welcome Mary!
```
<br>
**No React**, é igual, só que usamos **JSX.**

```jsx
// function components, JSX

function greeting(props) {
  return <h1>Hi, welcome {props.name}!</h1>;
}

<greeting name="Mary" />; // Hi, welcome Mary!
```
<br>

---

### **Funcionamento das `props`**

- São somente leitura (imutáveis)
- **`props`** não podem ser alteradas dentro do componente — o pai controla seus valores.

#### Modo convencional

- ##### **Components pai `JSX` passando atributos (`props`) ao component filho**

```jsx

<greeting title="greeting" age={50} active={true} />

```
<br>
- ##### **Components filhos recebendo atributos (`props`) como Object** 

```jsx
functions greeting(props) {

    const myProps = props; // { title: "greeting", age: 50, active: true }

    return (
        <>
        <p>Title: {props.title}</p>
        <p>Age: {props.age}</p>
        <p>Active: {props.active}</p>
        </>
    );
}

export {greeting};
```
<br>

---

#### Modo destruct (forma moderna)

Em vez de usar props.title, props.age, etc.,
podemos desestruturar o objeto props <br>

```jsx
function greeting({ title, age, active }) {
  return (
    <>
      <p>Title: {title}</p>
      <p>Age: {age}</p>
      <p>Active: {active}</p>
    </>
  );
}

export { greeting };
```

<br>

---

**Conclusão**

-  **Props** (parâmetros), que o pai envia ao filho 
-  **São imutáveis** (não podem ser alteradas internamente) 
-  São usadas para **comunicação entre componentes** 
-  Podem conter qualquer tipo de dado (**string, número, array, função, JSX, etc.**) 