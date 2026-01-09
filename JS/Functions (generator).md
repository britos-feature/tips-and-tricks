
---

# **Generator functions**  - ( funções geradora)

## O que é uma função geradora?

Uma **função geradora** é um tipo especial de função em JavaScript que pode **pausar e retomar sua execução**, produzindo valores sob demanda ao longo do tempo. Ela é definida com a palavra‑chave `function*` e utiliza `yield` para devolver valores intermediários.

Diferente de uma função comum, que executa todo o seu corpo de uma vez e retorna um único valor, uma função geradora retorna um **iterador**, permitindo controlar a execução passo a passo.

---

## Sintaxe básica

```js
function* exemplo() {
  yield 1;
  yield 2;
  yield 3;
}
```

Ao chamar a função, o código **não é executado imediatamente**:

```js
const gen = exemplo();
```

O valor retornado é um **Generator Object**.

---

## O método `.next()`

A execução da função geradora acontece quando chamamos o método `.next()`:

```js
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }
```

Cada chamada a `.next()`:

- continua a execução a partir do último `yield`
    
- retorna um objeto `{ value, done }`
    

---

## A palavra‑chave `yield`

O `yield` funciona como um **ponto de pausa**:

- entrega um valor ao consumidor
    
- suspende a execução da função
    
- preserva o estado interno (variáveis, loops, contexto)
    

```js
function* contador() {
  let i = 0;
  while (true) {
    yield i++;
  }
}
```

---

## `yield` vs `return`

| `yield`                   | `return`              |
| ------------------------- | --------------------- |
| Pausa a função            | Finaliza a função     |
| Pode ocorrer várias vezes | Ocorre apenas uma vez |
| Mantém o estado           | Descarta o estado     |

---

## Funções geradoras são iteráveis

Geradores implementam o protocolo de iteração e funcionam com:

- `for...of`
    
- spread operator (`...`)
    
- `Array.from()`
    

```js
for (const valor of exemplo()) {
  console.log(valor);
}
```

---

## Enviando valores para dentro do gerador

É possível enviar valores de volta para a função geradora usando `next(valor)`:

```js
function* conversa() {
  const nome = yield 'Qual é seu nome?';
  const idade = yield 'Qual é sua idade?';
  return `${nome} tem ${idade} anos`;
}
```

---

## `yield*` (delegação)

O `yield*` permite delegar a execução para outro iterador ou gerador:

```js
function* a() {
  yield 1;
  yield 2;
}

function* b() {
  yield* a();
  yield 3;
}
```

---

## Controle avançado

### `.throw()`

Lança um erro dentro do gerador:

```js
gen.throw(new Error('Erro interno'));
```

### `.return()`

Finaliza o gerador imediatamente:

```js
gen.return();
```

---

## Casos de uso comuns

- Paginação de dados
    
- Processamento lazy (sob demanda)
    
- Streams de dados
    
- Máquinas de estado
    
- Iteradores personalizados
    

---

## Considerações de desempenho

- Geradores são **mais lentos por iteração** que funções normais
    
- Usam **menos memória**, pois produzem valores conforme necessário
    
- Ideais para grandes volumes de dados ou sequências infinitas
    

---

## Resumo

> Uma função geradora é uma função pausável que produz valores progressivamente, oferecendo controle fino sobre o fluxo de execução e uso eficiente de memória.

Funções geradoras são uma poderosa ferramenta para modelar processos iterativos, fluxos complexos e dados sob demanda em JavaScript.

