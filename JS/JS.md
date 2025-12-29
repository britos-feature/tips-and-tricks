# Java Script
JavaScript é uma linguagem de programação de alto nível, interpretada e baseada em eventos, amplamente utilizada para o desenvolvimento web. É uma das principais tecnologias da web, ao lado de HTML e CSS, permitindo criar páginas interativas e dinâmicas. 
**[[#Atribuição Java Script ao HTML| Forma de inserir JS em seu projeto]]**


## Referências
### [[Variables]], [[Strings]], [[Number]], [[Short-circuit]], [[Arrays]], [[Objects]], [[Functions]], [[Class]], [[Promises]], [[JSON]]


## Utilidades para utilização de Java Script em um projeto:

- Criação de interatividade –> Permite manipular elementos da página (DOM) "browser", responder a eventos do usuário e criar animações.

- Desenvolvimento de aplicações web –> É a base de frameworks como React, Angular e Vue.js.

- Back-end com Node.js – Possibilita o desenvolvimento de servidores e APIs usando JavaScript.

- Desenvolvimento de jogos – Pode ser usado para criar jogos web com HTML5 e bibliotecas como Phaser.

- Aplicações mobile e desktop – Frameworks como React Native e Electron permitem criar aplicativos para diversas plataformas.


## Atribuição Java Script ao HTML
No HTML pode-se atribuir arquivos JavaScript de três maneiras principais: **inline**, **interno** e **externo**.
- **inline** -> JScript "inline" (no próprio HTML), O código JavaScript é colocado diretamente dentro de atributos **HTML**, como `onclick`, `onmouseover`, etc...
<br>
- **interno** -> JScript "interno" (dentro do HTML), O código JavaScript é inserido dentro da tag `<script>` no próprio arquivo HTML. 

	> 	*Melhor organização do que o inline, mas ainda pode dificultar a manutenção em projetos grandes.*

-  **externo** -> JScript "externo" (arquivo separado), O código é escrito em um arquivo `.js` separado e referenciado no HTML usando a tag `<script>`.

	>	*Melhor prática para organização, manutenção e reutilização de código.  Permite que o JavaScript seja carregado de forma assíncrona com os atributos **defer** ou **async**.


### Atribuições:

```html
<!-- atribuição inline -->
<button onclick="alert('Você clicou no botão!')">Clique aqui</button>
```


```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Interno</title>
</head>
<body>

    <button id="meuBotao">Clique aqui</button>

<!-- atribuição interno -->
    <script>
        document.getElementById("meuBotao").addEventListener("click", function() {
            alert("Você clicou!");
        });
    </script>

</body>
</html>
```


```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Externo</title>
<!-- atribuição externa (arquivo separado) -->
	<script src="script.js" defer></script> 
</head>
<body>

    <button id="meuBotao">Clique aqui</button>

</body>
</html>

```

```js
// arquivo externo JS 
document.getElementById("meuBotao").addEventListener("click", function() {
    alert("Você clicou!");
});
```

 **Dica sobre `defer` e `async`**
 
 -  **`defer`**: Garante que o JS será carregado **após** o HTML ser completamente lido, mantendo a ordem de execução.

- **`async`**: Carrega o JS de forma assíncrona, sem esperar pelo HTML, o que pode causar problemas de dependência. Isso é útil para scripts que não dependem do DOM ou de outros scripts para funcionar corretamente.

==!!!  Sempre que possível, use **arquivos JS externos com  defer**, pois melhora a organização e o desempenho do site==




