
- ## Lógica dos operadores && e  || 


```js

const numberTrue =  1 && 2 && && 3;
console.log(numberTrue); // Aqui todos valores são 'true', nesse caso será retornado o último valor 'true'. 


const numberFalse = 1 && 2 && 0 && 3;
console.log(numberFalse); // Aqui encontrado um valor 'false', o mesmo será retornado.

//----------------------------------------------------------------------

const letterTrue = 'a' || 'b' || 'c'; 
console.log(letterTrue); // Utilizando esse operador, o primeiro valor 'true' será retornado.

const letterFalse = null || 0 || 'c' || 'd';
console.log(letterFalse); // Aqui será retornado o letter 'c', pois todos os outros antes tinham seus valores com 'false'

```


- ## Destruct (destruturação)

```js

const dados = { name: 'Alex', lastname: 'Brito', email: 'alex@email.com' };

const newDados = { ...dados, phone: 11996650222 };
}

console.log(newDados);
/** return {
name: Alex,
lastname: Brito
email: alex@eamil.com,
phone: 11996650222
}

```

> Útil para requisições (testar recebimento de valores).

```js
const { name, lastname, email, phone } = req.body;

//Sequelize atualização(update), onde values 'undefined' são ignorados no UPDATE
const valueDefault = {
	...(name !== undefined && { name }),
	...(lastname !== undefined && { lastname }),
	...(email !== undefined && { email }),
	...(phone !== undefined && { phone }),
}

// * valueDefault = é um Object ja com chaves e valores definidos.
 
// * Atualização:
/*
 * - Variável, valor não recebido, o mesmo não será atualizado, permanecendo o mesmo ja existente.
 * - Variável, valor recebido, Object será atualizado repassando o valor a chave correspondente do object.

```




