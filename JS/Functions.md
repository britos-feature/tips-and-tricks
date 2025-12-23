# TUDO SOBRE FUNÇÕES JS

No JavaScript, a escolha entre **CONST**,**'FUNCTTION(tradicionais)**,**'FUNCTION ANONYMAL** e **ARROW-FUNCTION** geralmente tem mais a ver com clareza e estilo de código do que com desempenho.

#### `CONST`

- **Descrição**: **CONST (CONSTANTE)** é usado para declarar variáveis que não devem ser reatribuídas. Isso significa que você não pode reatribuir o valor da variável, mas pode modificar o conteúdo de objetos e arrays.

- **Desempenho**: Não afeta diretamente o desempenho. A principal vantagem é evitar a reatribuição acidental de variáveis e melhorar a previsibilidade do código.

#### FUNCTION

- **Descrição**: **FUNÇÃO TRADICIONAL** são declaradas usando a sintaxe **_`function`_** e podem ser usadas para criar métodos e funções que têm um contexto (`this`) próprio.

- **Desempenho**: Pode ser ligeiramente mais lento em alguns casos porque o contexto (`this`) precisa ser gerenciado. No entanto, isso raramente é um problema perceptível para a maioria das aplicações.

#### FUNCTION ANONIMAL

- **Descrição**: **FUNÇÕDS ANONIMAS** são funções sem nome que podem ser atribuídas a variáveis. Elas são frequentemente usadas como **callbacks**.

- **Desempenho**: O desempenho é similar ao das funções tradicionais. A principal diferença é que, por serem anônimas, podem ser menos legíveis em alguns casos.

#### ARROW-FUNCTION

- **Descrição**: **ARROW-FUNCTION** são uma forma concisa de escrever funções. Elas não têm seu próprio **`this`** e, em vez disso, herdam o **`this`** do contexto onde foram criadas. São especialmente úteis para funções de uma linha ou funções passadas como callbacks.

- **Desempenho**: Em geral, não há uma diferença significativa de desempenho em comparação com funções tradicionais. A principal vantagem é a sintaxe mais compacta e a herança léxica do `this`, que pode simplificar o código e reduzir bugs relacionados ao contexto.

##### Resumo:

1. **CONST**: Útil para garantir que uma variável não seja reatribuída, mas não afeta o desempenho.

2. **FUNCTION**: Podem ser um pouco mais lentas devido à manipulação do `this`, mas a diferença é mínima.

3. **FUNCTION ANONIMAL**: Semelhantes às funções tradicionais em termos de desempenho, mas menos legíveis se não forem usadas com cuidado.

4. **ARROW-FUNCTION**: Oferecem uma sintaxe mais limpa e ajudam a evitar problemas com o `this`, sem uma penalidade significativa de desempenho.

> No geral, a escolha entre essas opções deve ser guiada mais por legibilidade e mantenabilidade do código do que por preocupações de desempenho. Em casos críticos de desempenho, você pode precisar fazer medições e testes específicos, mas para a maioria das aplicações, qualquer diferença de desempenho entre essas opções é mínima.

### TIPOS DE FUNÇÕES

1. **BASIC FUNCTION**
   Em JavaScript, uma função é um bloco de código reutilizável que pode ser executado quando chamado. A função serve para encapsular uma lógica que pode ser reutilizada várias vezes ao longo do código, permitindo que você evite duplicação e mantenha o código mais organizado e modular.

#### Função básica

**Sintaxe:**

```js
// declaração de função basica
function myFunctionBasic(x, y) {
  x + " " + y; // concatena os valores recebidos (não retornando nada)
}

// execução da função declarada
FunctionBasic("Hello", "World");
```

> Uma função tem como sua única obrigação a executar do bloco de comando passado. Caso queira que a função retorne algum valor, é necessário informar a função o seu retorno.

#### Anonymous function

Função Anonima, é uma função que não tem um nome atribuído. Em vez de declarar uma função com um nome como em uma função regular, a função anônima é definida diretamente no local em que será usada, e geralmente é atribuída a uma variável ou passada como argumento para outra função.

**Caracteristicas:**

- Não têm nome.
- São frequentemente usadas como callbacks (funções passadas como parâmetros para outras funções).
- Podem ser atribuídas a variáveis ou constantes.
- Usadas para encapsular um bloco de código que só será executado uma vez ou que precisa ser passado como argumento.
- **THIS** corresponde ao chamador da função.

**Sintaxe:**

```js
// delcaração de função anônima
const myFunctionAnonymal = function (x, y) {
  return x, " ", y;
};

// execução da função declarada
myFunctionAnonymal("Hello", "World");
// return Hello World
```

#### Arrow-function

Uma Arrow function em JavaScript é uma forma concisa de escrever funções, introduzida no ES6. Ela tem uma sintaxe mais curta e um comportamento diferente em relação ao uso do valor de 'THIS' em comparação com as funções tradicionais.

**Diferenças importantes das Arrow functions:**

- **_THIS léxico_**, maior diferença entre uma arrow function e uma função tradicional é que a Arrow function não tem seu próprio **'THIS'**.
- **_THIS_** corresponde ao valor GLOBAL

**Sintaxe:**

```js
// declaração de função Arrow-function
const myArrowFunction = (x, y) => x + " " + y;

// execução da função arrow-function
myArrowFunction("Hello", "World");
// return Hello World
```

#### Factory function

Uma Factory Function em JavaScript é uma função que cria e retorna objetos. Em vez de usar a palavra-chave **_class_** ou **_new_** para criar instâncias de objetos, a **\*factory function\*\*** é uma função regular que cria e configura objetos dinamicamente e os retorna.

**_Características principais de uma Factory Function:_**

- **Criação dinâmica de objetos:** A função pode criar diferentes objetos a partir dos parâmetros fornecidos.

- **Não usa 'NEW' ou 'THIS' global:** Diferentemente dos construtores de objetos (usando classes ou funções construtoras com NEW),

- As **factory functions** retornam explicitamente um objeto, sem a necessidade de usar new.

- **Fábrica de objetos:** O termo "factory" (fábrica) vem da ideia de que essas funções são usadas para fabricar, ou criar, objetos de maneira repetível.

- **Encapsulamento:** Factory functions podem encapsular a lógica de criação, permitindo a personalização e a criação de objetos de maneira mais flexível. Isso pode incluir adicionar métodos, propriedades privadas, ou retornar diferentes tipos de objetos com base nas entradas.

**Vantagens de usar Factory Functions:**

- **Flexibilidade:** As factory functions podem retornar diferentes tipos de objetos ou adicionar lógica condicional de maneira simples.

- **Encapsulamento:** Elas podem esconder detalhes de implementação dentro da função, tornando os objetos retornados mais seguros.

- **Facilidade de uso:** São mais simples e intuitivas do que classes, especialmente para criar objetos dinâmicos ou quando você não precisa de herança.

> OBS:. OBRIGATÓRIO INSTANCIAR A FUNÇÃO E DEPOIS EXECUTA-LA PARA RETORNO DE SEU VALORES/ATRIBUTOS

**Sintaxe:**

```js
// declaração de função factory function
function myFactoryFunction(name, lastname) {
  return {
    // attributes
    name,
    lastname,

    // methods (função dentro de função)
    greeting: function () {
      return `${this.name} ${this.lastname}`;
    },
  };
}

// execução da função factory function
const myFactory = myFactoryFunction("Hello", "World");
// return { name: 'Hello', lastname: 'World', greeting: [Function: greeting] }
```

#### Construct function

Em JavaScript, a **função construtora** (ou "Construct function") é uma função especial usada para criar e inicializar objetos. Ela permite que você crie vários objetos similares a partir de uma "molde" ou "classe".

**Principais características de uma função construtora:**

- **Nomeação com letra maiúscula:** Por convenção, o nome de uma função construtora começa com letra maiúscula para diferenciá-la de outras funções.

- **Uso de 'THIS':** Dentro da função construtora, 'THIS' é usado para referir-se ao novo objeto que está sendo criado.

- **Uso de 'NEW':** Para invocar uma função construtora, você deve usar a palavra-chave 'NEW'. Isso faz com que um novo objeto seja criado e vinculado ao 'THIS' dentro da função.

- **Métodos definidos no protótipo são compartilhados entre todas as instâncias.**

- Mais eficiente em termos de memória, pois métodos são definidos no protótipo ao invés de serem recriados para cada instância.

- Function criadas dentro de uma CONSTRUCT FUNCTION ela são chamadas de **METHOD.**

**Formas de declarações de Methods dentro da função:**

- **nameAttribute:** function name() {};
- **nameAttribute:** function() {};
- **nameAttribute:** () => {};

**Sintaxe:**

```js
// declaração de Construct function
function MyConstructFunction(x, y) {
  // attribute
  this.name = x;
  this.lasname = y;

  // Methods (PUBLIC)
  // Function basic
  this.functionBase = function functionBase() {
    this.name + " " + this.lastname;
  };

  // Anomymous function
  this.anonymousFunction = function () {
    return `${this.name} ${this.lasname}`;
  };

  // Arrow function
  this.arrowFunction = () => `${this.name} ${this.lasname}`;

  // Method (RESTRICT)
  const functionBase2 = function () {
    `${this.name} ${this.lasname}`;
  };

  baseFunction.call(this); // chamada ao Method restrict refenciando o 'THIS' ao object instanciado.

  baseFunction.apply(this); // chamada ao Method restrict refenciando o 'THIS' ao object instanciado.

  // Method (restrict com 'bind')
  const functionBase3 = function () {
    return `${this.name} ${this.lasname}`;
  }.bind(this);
  functionBase3(); // chamada ao Method restrict com 'bind()' referenciando o 'THIS' ao object instanciado.

  // Method (restrict 'arrow function')
  const arrowFunction = () => `${this.name} ${this.lasname}`;
  arrowFunction(); // chamada ao Method restrict com 'Arrow function()' refenciando o 'THIS' ao object instanciado.

  /*
Modo restrict precisam ser utilizados por esses modos para ter acesso ao 'THIS', object instanciados (explicação):
     
	 * Arrow functions -> herdam o contexto léxico de this, o que resolve automaticamente o problema de escopo de 'THIS'.
     
	 * bind -> pode ser usado para associar explicitamente 'THIS' à função, garantindo que ela sempre use o contexto correto.
     
	 * call e apply ->  podem ser usados quando você precisa passar o contexto de this manualmente no momento da chamada.
*/

  // Essas soluções garantem que você possa acessar corretamente 'this.name' e 'this.lasname' na função baseFunction.
}

// execução/ instanciação da Construct function
const constructFunction = new ConstructFunction("Alex", "Brito"); // instancia Construct function -> molde (class)
```

### PROTOTYPES- PROTÓTIPOS

**Protótipos** => Em JavaScript, **protótipos** referem-se ao sistema de herança que permite que objetos herdem propriedades e métodos de outros objetos.

> Todo objeto em JavaScript tem uma propriedade interna chamada [[Prototype]],

**Prototypes**
É uma referência a outro objeto, essa referência é utilizada para buscar propriedades e métodos que não estão diretamente definidos no objeto em questão.

**Resumo:**

- Os protótipos em JavaScript são uma parte fundamental de como a linguagem implementa a herança e a reutilização de código.

- Eles permitem que objetos compartilhem métodos e propriedades de forma eficiente, evitando a duplicação de código e promovendo a organização.

**OBJECT PROTOTIPO:** Quando você tenta acessar uma propriedade em um objeto e ela não existe, o JavaScript verifica o objeto protótipo em busca dessa propriedade. Essa cadeia de protótipos é conhecida como "cadeia de protótipos" (PROTOTYPE CHAIN).

**Propriedade PROTOTYPE:**
Funções em JavaScript (especialmente CONSTRUCT FUNCTION) possuem uma propriedade chamada 'PROTOTYPE', que é um objeto que contém as propriedades e métodos que serão herdados pelos objetos criados a partir dessa função.

**Exemplo:**
Se você tiver uma 'CONSTRUCT FUNCTION' Car, os METHODS que você adicionar ao Car.prototype, estarão disponíveis para todos os objetos criados a partir da função Car.

Prototype Chain (cadeia de proto) -> Contruct function (default)
-> object intance (corpo)
-> construct function (object)
-> object.prototype

Prototype Chain (cadeia de proto) -> Object literal (default)
-> object literal(corpo)
-> Object contruct (Object.prototype)

> OBS. Lembrando que um OBJECT literal é o mesmo que OBJECT Construt

```js
// declaração de um OBJECT literal
var obj = { name: "Alex", lastname: "Brito" };

// Declaração de um Construct OBJECT
var obj = new Object();
obj.name = "Alex";
obj.lasname = "Brito";
```

#### Method getPrototypeOf()

Esse método retorna o prototype (isto é, o valor da propriedade interna [[Prototype]]) do objeto especificado.

```js
const proto = Object.getPrototypeOf(obj);
```

#### Method setPrototypeOf()

Esse método configura o 'prototype' (i.e., a propriedade interna [[Prototype]]) de um objeto específico para outro objeto

**Sintaxe:**

```js
Object.setPrototypeOf(obj, prototype);
// obj = O objeto que deve ter seu 'prototype' definido.
// prototype = O novo 'prototype' para o objeto (um outro objeto ou null).

const objA = {
  chaveA: "A", // cadeia default prototype (corpo objA, object construct[prototype])
};

const objB = {
  chaveB: "B", // cadeia default prototype (corpo objB, object construct[prototype])
};

const objC = {
  chaveC: "C", // cadeia default prototype (corpo objC, object contruct[prototype])
};

// Method setPrototypeOF() -> configurando o prototype (propriedade interna), para HERANÇA aos valores de outro obj
Object.setPrototypeOf(objB, objA); // 'objB' herdando acesso aos valores do 'objA'

objB.chaveB; // return 'B';
objB.chaveA; // return 'A';

Object.setPrototypeOf(objC, objB); // objC herdando acesso aos valores do objA e ObjB (alterado anteriormente)

objC.chaveC; // return 'C';
objC.chaveB; // return 'B';
objC.chaveA; // return 'A';
```

### Compartilhamento de métodos e propriedades PROTOTYPE

Uma forma eficiente descartando a duplicação de código e promovendo a organização.

**Herança e a reutilização de código**

- Processo Herança e a reutilização de código (Construct function PROTOTYPE).
- Create Methods dentro do PROTOTYPE of (Construct function), faz com que todas as INSTANCE De FUNCTION tenham acesso as Method criado, motivo de Herança de prototype

```js
// Construct function
function Car(brand, model, year, color) {
  this.brand = brand;
  this.model = model;
  this.year = year;
  this.color = color;
}

// compartilhamento de método e propriedades via Prototype
Car.prototype.description = function () {
  return `Description: Brand: ${this.brand}, Model: ${this.model}, Year: ${this.year}, Color: ${this.color}`;
};

// exececição / instance (Construct function)
const onix = new Car("Chevrolet", "Onix LTS", 2024, "black");
onix.description();
// return description object (Method de herança devido o prototype herdado da COnstruct function).
```

#### Herança - Prototype

Henrança em JS também se faz uso dos PROTOTYPE

> Lembrando-se que o método Object.create() cria sempre um objetc vazio (empty)

**Utilizando Herança no JS**

```js
// Create Construct function (base)
function Product(name, price) {
  this.name = name;
  this.price = price;
}

// Create Method for PROTOTYPE Construct function (Product)
Product.prototype.priceIncrease = function (percent) {
  return this.price + (this.price * percent) / 100;
};

Product.prototype.priceDiscount = function (percent) {
  return this.price - (this.price * percent) / 100;
};

// criação de função construtura para produtos especificados, utilizando-se de HERANÇA do construtor (base)
function Shorts(name, price, color) {
  //Method call() utilizado para substituir anotações do object pela as anotações do object atual
  // this = object que chamou o method <->  name, price = lista de argumento passado ao method */
  Product.call(this, name, price);

  // attribute
  this.color = color;
}

// Todo Construct tem um PROTOTYPE(OBJECT empty), onde podemos adicionar Methods/Attributes para compartilhamento/herança.

// Essa sintaxe adiciona o PROTOTYPE de outro OBJECT no PROTOTYPE refenciado.
Shorts.prototype = Object.create(Product.prototype);

// SINTAXE que adiciona o CONSTRUCT de outro OBJECT no CONSTRUCT referenciado.
Shorts.prototype.construct = Shorts;

// Instance by Construct (Shorts)
const shorts = new Shorts("regata", 69, "black");
```
