Ativando **destaque sintaxe (highlight)** em arquivos com a extensão **`.sequelize`**

Como essa extensão não é reconhecida por padrão, o VS Code não sabe que tipo de linguagem aplicar — mas dá pra corrigir isso facilmente.

---

## Opção 1 — Fazer isso rapidamente pelo comando

Você também pode usar o **Comando Rápido**:

1. Abra o arquivo `.sequelize`.
2. No canto inferior direito do VS Code, clique onde aparece algo como **“Plain Text”**.
3. Escolha **“Configure File Association for ‘.sequelize’…”**
4. Depois selecione **“JavaScript”** na lista.

Pronto, todos os arquivos `.sequelize` serão tratados como JavaScript automaticamente.

---

## Opção 2 — Associar `.sequelize` a JavaScript no próprio VS Code

1. Abra o **VS Code**.
2. Vá em **Configurações (Ctrl + ,)** → pesquise por `files.associations`.
3. Clique em **“Editar em settings.json”** (ícone de abrir arquivo JSON).
4. Adicione esta linha dentro do objeto:
 
```json

"files.associations": {
	"*.sequelize": "javascript"
}

```

6. Salve o arquivo.   
7. Reabra o arquivo `.sequelize` — agora o _highlight_ de **JavaScript** deve funcionar normalmente.

---

## Dica extra (para sintaxe ES6, Sequelize, etc.)

Se quiser realce ainda melhor, instale essas extensões no VS Code:

- **Babel JavaScript** — melhora o _highlight_ de ES6, import/export e classes.
- **Sequelize ORM Snippets** — adiciona _snippets_ e reconhecimento de código do Sequelize.

---

