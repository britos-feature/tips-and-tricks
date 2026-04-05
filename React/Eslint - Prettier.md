
---

## Guia prático para criação de um projeto React (CRA) com ESLint + Prettier usando o padrão Airbnb.

---

## Criar o projeto com CRA

```bash
npm create-react-app . # name project '.'
```

---

# Eslint

O **ESLint** é uma **ferramenta de análise de código (linter)** usada principalmente em **JavaScript e TypeScript**.

Em termos simples:  
👉 ele **verifica seu código automaticamente** para encontrar **erros**, **más práticas** e **inconsistências de estilo**, antes mesmo do código rodar.

---

## Para que o ESLint serve?

Ele ajuda a:

- ❌ Evitar **bugs comuns**
- 📏 Manter um **padrão de código**
- 🧹 Detectar **variáveis não usadas**
- ⚠️ Apontar **erros silenciosos** (ex.: código que “funciona”, mas está errado)
- 👥 Facilitar trabalho em **equipe**

---
## Instalar dependências do ESLint + Airbnb + Prettier

```bash
npm install --save-dev \
eslint-config-airbnb \
eslint-plugin-import \
eslint-plugin-jsx-a11y \
eslint-plugin-react \
eslint-plugin-react-hooks \
eslint-config-prettier \
eslint-plugin-prettier \
prettier
```

> O CRA já vem com ESLint instalado, então não instale eslint novamente.

---

## Configurar ESLint

Crie ou edite o arquivo .eslintrc.json na raiz do projeto

```bash
touch .eslintrc.json
```

- Content file **`.eslintrc.json`**

```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "airbnb",
    "airbnb/hooks",
    "plugin:prettier/recommended"
  ],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "react/react-in-jsx-scope": "off",
    "import/prefer-default-export": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }]
  }
}
```

---

# Prettier

O **Prettier** é uma **ferramenta de formatação automática de código**.

Em poucas palavras:  
👉 ele **organiza o visual do código** para que tudo fique consistente, **sem mudar a lógica**.

---

## Para que serve o Prettier?

Ele cuida apenas da **aparência do código**, como:

- 📐 Indentação
- ✂️ Quebras de linha
- ❝ Aspas simples ou duplas
- ➕ Espaços
- ➖ Ponto e vírgula
- 📏 Tamanho máximo de linha

Você escreve o código do jeito que quiser, e o Prettier **reorganiza tudo sozinho**.

---
## Configurar Prettier

Crie o arquivo **`.prettierrc`**

```bash
touch .prettierrc
```

- Content file **`.prettierrc`**

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "trailingComma": "es5",
  "tabWidth": 2
}
```

---

## Evitar conflitos ESLint × Prettier

Crie .eslintignore

```bash
touch .eslintignore
```

- Content file **`.eslintignore`**

```nginx
node_modules
build
```

> (opcional, mas recomendado)

---

## Scripts úteis (opcional)

No **`package.json`**, você pode adicionar

```json
"scripts": {
  "lint": "eslint src",
  "lint:fix": "eslint src --fix",
  "format": "prettier --write \"src/**/*.{js,jsx,json,css,md}\""
}
```

---

## Integração com VS Code (recomendado)

Instale as extensões **_(ESLint, Prettier - Code formatter)_**

E adicione estas configurações no settings.json do VS Code

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

---

## Regras Airbnb mais ajustadas (React moderno)

Exemplo de .eslintrc.json ajustado

```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "airbnb",
    "airbnb/hooks",
    "plugin:prettier/recommended"
  ],
  "rules": {
    /* === Prettier === */
    "prettier/prettier": "error",

    /* === React === */
    "react/react-in-jsx-scope": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
    "react/prop-types": "off",
    "react/function-component-definition": "off",

    /* === Imports === */
    "import/prefer-default-export": "off",
    "import/extensions": "off",

    /* === JavaScript === */
    "no-console": "warn",
    "no-unused-vars": ["warn", { "argsIgnorePattern": "^_" }],
    "class-methods-use-this": "off",

    /* === Estilo === */
    "arrow-body-style": "off",
    "object-curly-newline": "off"
  }
}
```

---