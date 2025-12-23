# Class JS
No JavaScript moderno (ES6+), a palavra-chave **`class`** é usada para criar classes, que são modelos para a criação de objetos. Isso facilita a reutilização de código e melhora a organização do programa.

Antigamente, JavaScript usava funções construtoras para criar objetos. Com a introdução das classes, ficou mais intuitivo trabalhar com a programação orientada a objetos (POO).

## Criando um `class`

```js
class Peoples {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greeting() {
    return `Olá, meu name é ${this.name} e tenho ${this.age} anos.`;
  }
}

// Criando um objeto (instância) da classe
const person = new Peoples("Alex", 49);

console.log(person.greeting());
// Saída: "Olá, meu name é Alex e tenho 49 anos."

```


#### Explicação:

- **`class Peoples`** → Define a classe `Pessoas`.
- **`constructor(name, age)`** → O método especial `constructor` é chamado automaticamente quando um new objeto é criado.
- **`this.name = name`** e **`this.age = age`** → Definem propriedades dentro do objeto.
- **`Método greeting()`** → É um comportamento que pode ser chamado nos objetos criados.


---
---

## Métodos de Instância / Métodos Estáticos em JavaScript
Em classes JavaScript, os métodos podem ser de dois tipos:

- **Métodos de instância** → Pertencem a um objeto específico criado a partir da classe.
- **Métodos estáticos (`static`)** → Pertencem diretamente à classe e não às instâncias.

#### Métodos de Instância
Os **métodos de instância** são aqueles que precisam ser chamados em um objeto criado a partir da classe. Eles têm acesso às propriedades do objeto usando `this`.

```js
class Peoples {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // Método de instância
  greeting() {
    return `Olá, meu nome é ${this.name} e tenho ${this.age} anos.`;
  }
}

// Criando instâncias
const person1 = new Peoples("Ana", 28);
console.log(person1.greeting()); // "Olá, meu nome é Ana e tenho 28 anos."

```

#### Características dos métodos de instância:

✔ Precisam ser chamados em um objeto (instância da classe).  
✔ Podem acessar `this` para manipular os atributos do objeto.  
✔ São o tipo mais comum de método dentro de classes.


#### Métodos Estáticos (`static`)
Os **métodos estáticos** pertencem à classe e não a objetos individuais. Isso significa que eles **não podem acessar `this`**, pois `this` representa uma instância.

```js
class Mathematics {
  // Método estático
  static sum(a, b) {
    return a + b;
  }
}

// Chamando o método estático
console.log(Mathematics.sum(5, 10)); // 15


// Tentativa de chamar em um objeto falha
const calc = new Mathematics();
// console.log(calc.sum(2, 3));
// ❌ ERRO! Métodos estáticos não podem ser chamados por instâncias.

```

#### Características dos métodos estáticos:

✔ São chamados diretamente na classe (`Classe.metodo()`).  
✔ **Não acessam `this`**, pois pertencem à classe, não a uma instância.  
✔ São úteis para funções auxiliares ou utilitárias (como cálculos).

> **Obs:** Os Métodos de **`instância`** e **`estáticos`** se necessário podemos te-los na mesma class.


### Quando usar cada um do métodos ?

| Tipo de Método | Como é chamado    | Acessa `this`? | Quando usar?                                                                                           |
| -------------- | ----------------- | -------------- | ------------------------------------------------------------------------------------------------------ |
| **Instância**  | `obj.metodo()`    | ✅ Sim          | Quando precisar manipular propriedades do objeto                                                       |
| **Estático**   | `Classe.metodo()` | ❌ Não          | Quando não precisar acessar propriedades da instância (funções auxiliares, cálculos, validações, etc.) |


---
---


## Herança: Extending Classes
A herança permite que uma classe herde propriedades e métodos de outra classe usando `extends`.

```js
class Employee extends Peoples {
  constructor(name, age, position) {
    super(name, age); // Chama o construtor da classe "Pai" (Pessoa)
    this.position = position;
  }

  greeting() {
    return `${super.greeting()} Eu trabalho como ${this.position}.`;
  }
}

// Criando um objeto
const employee1 = new Employee("Carlos", 40, "Desenvolvedor");
console.log(employee1.greeting());
// Saída: "Olá, meu name é Carlos e tenho 40 anos. Eu trabalho como Desenvolvedor."

```


#### Explicação:

- **`class Employee extends Peoples`** → A classe Employee herda de Peoples.
- **`super(name, age)`** → Chama o `constructor` da classe `Peoples`.
- **Sobrescrevendo o método `greeting()`** → Usa `super.greeting()` para reutilizar a lógica da classe pai.


---
---

## Métodos `get` e `set`
Os métodos **`get`** e **`set`** permitem manipular propriedades de forma controlada.

```js
class Product {
  constructor(name, price) {
    this.name = name;
    this._price = price; // Convenção: usar "_" indica que é um campo "privado"
  }

  get price() {
    return `R$ ${this._price.toFixed(2)}`;
  }

  set price(newPrice) {
    if (newPrice > 0) {
      this._price = newPrice;
    } else {
      console.log("O preço deve ser maior que zero.");
    }
  }
}

// Criando um product
const product1 = new Product("Celular", 1500);
console.log(product1.price); 
// "R$ 1500.00"

product1.price = 2000; 
// Define um new preço

console.log(product1.price); 
// "R$ 2000.00"

product1.price = -50; 
// Tentativa inválida
// "O preço deve ser maior que zero."

```


#### Explicação:

- **`get preco()`** → Um método getter para acessar `_preco`.
- **`set preco(novoPreco)`** → Um setter para modificar `_preco`, mas só aceita valores positivos.


---
---

## Membros Privados (`#`)
A partir do ES2020, podemos usar `#` para definir propriedades realmente privadas dentro da classe.

```js
class BanckAccount {
  #balance; // Propriedade privada

  constructor(balanceInitial) {
    this.#balance = balanceInitial;
  }

  deposit(value) {
    if (valuw > 0) {
      this.#balance += value;
    }
  }

  get balance() {
    return this.#balance;
  }
}

const myAccount = new BanckAccount(1000);
minhaConta.deposit(500);
console.log(myAccount.balance);
// 1500

// console.log(myAccount.#balance); 
// ❌ ERRO: Propriedade privada não pode ser acessada diretamente.

```


#### Explicação:

-  **`#balance`** → Define uma propriedade privada que só pode ser acessada dentro da classe.
-  **Método `deposit(value)`** → Modifica o saldo sem permitir acesso direto à variável.


#### Resumo

✅ **Classes** organizam código e permitem a reutilização através da herança.  
✅ **Métodos** dentro de classes representam comportamentos dos objetos.  
✅ **Herança (`extends`)** permite criar subclasses que herdam características da classe pai.  
✅ **`super()`** chama métodos da classe pai dentro da classe filha.  
✅ **Getters e Setters** controlam o acesso às propriedades.  
✅ **Propriedades privadas (`#`)** garantem encapsulamento real.


