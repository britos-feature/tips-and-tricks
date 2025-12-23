`setState()` é um **método herdado de `React.Component`** que serve para **atualizar o estado interno** (`state`) de um componente de classe.

#### **Entendimento**

> **`this.setState()`** diz ao React: “Atualize o estado e re-renderize o componente com os novos valores.”


#### **Estrutura básica**

```jsx

this.setState(updater, callback);

```
##### Parâmetros:

1. **_`updater`_** → o que você quer mudar no estado. Pode ser:    
    - um **objeto** com novos valores  
        → **`{ chave: novoValor }`**, ou uma **função** que recebe o estado anterior  
        → **`(prevState, props) => novoObjeto`**
        
2. **`callback`** → função opcional executada **depois que o estado é atualizado e o componente re-renderizou**.

---

#### **Modos de atualizar a variável**  **_`state`_**

- ##### **Atualizando diretamente**

```jsx
this.setState({ name: "Maria" });

```

> Isso **atualiza apenas** o campo `name` no `state`, mantendo os outros intactos.
<br>

**Exemplo de estado antes/depois:**

| Antes (`this.state`)        | Depois (`this.state`)        |
| --------------------------- | ---------------------------- |
| `{ name: "João", age: 25 }` | `{ name: "Maria", age: 25 }` |

---

- ##### **Atualizando com base no estado anterior**
	Quando o novo valor depende do estado antigo (ex: contador), use a **função**:

```jsx

this.setState((prevState) => ({
  count: prevState.count + 1,
}));

```

> Isso evita bugs causados pelo comportamento **assíncrono** de `setState()`.

---

- ##### **Usando o callback, mostra value atualizado após mudança**
	Se você quer **executar algo depois que o estado foi realmente atualizado**, use o segundo parâmetro:

```jsx

this.setState({ name: "Maria" }, () => {
  console.log("Novo nome:", this.state.name);
});

```

> Fora do callback, o valor pode **ainda não estar atualizado**.
> O segundo argumento (`() => { ... }`) é executado **após o React concluir a atualização** do estado e a re-renderização. É a maneira oficial e segura de acessar o valor novo.

##### **_`setState()`_** é **assíncrono**
- O React agrupa várias atualizações de estado para melhorar o desempenho (“**batching**”).
	- Isso significa que **`this.state` não muda imediatamente** após o `setState`.

---

#### **Atualização em lotes**

Se você fizer várias chamadas a `setState` seguidas, o React **pode agrupar** e atualizar tudo de uma vez para evitar renderizações desnecessárias.

```jsx

this.setState({ count: 1 });
this.setState({ name: "Maria" });

```

---

## Resumo final:

|Situação|Forma correta|
|---|---|
|Atualizar estado fixo|`this.setState({ nome: "Ana" })`|
|Atualizar baseado no estado anterior|`this.setState(prev => ({ count: prev.count + 1 }))`|
|Rodar algo depois da atualização|`this.setState({ x: 5 }, () => console.log(this.state.x))`|
|Impedir erro de valor antigo|Sempre usar o callback ou a função `prevState`|