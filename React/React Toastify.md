
---

# 📢 React Toastify

O **React Toastify** é uma biblioteca popular para exibição de **notificações (toasts)** em aplicações React de forma simples, elegante e altamente configurável. Ele permite informar o usuário sobre ações, erros, sucessos e alertas sem interromper o fluxo da aplicação.

---

## 🚀 Principais Vantagens

- Fácil de instalar e usar
- Totalmente customizável
- Suporte a animações
- Controle de posição, tempo e estilo
- Compatível com **TypeScript**
- Não depende de componentes complexos

---

## 📦 Instalação

Utilizando **npm**:

```bash
npm install react-toastify
```

Ou com **yarn**:

```bash
yarn add react-toastify
```

---

## 🛠️ Configuração Inicial

Antes de usar os toasts, é necessário importar o **ToastContainer** e o CSS padrão da biblioteca.

### Exemplo no arquivo principal (`App.jsx` ou `App.tsx`):

```jsx
import { ToastContainer } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';

function App() {
  return (
    <>
      {/* Conteúdo da aplicação */}
      <ToastContainer />
    </>
  );
}

export default App;
```

> ⚠️ O `ToastContainer` deve ser renderizado **apenas uma vez** na aplicação.

---

## 🔔 Como Usar

Após a configuração, você pode disparar notificações em qualquer componente.

### Importação básica:

```jsx
import { toast } from 'react-toastify';
```

### Exemplos de notificações:

```jsx
toast("Mensagem padrão");

toast.success("Operação realizada com sucesso!");

toast.error("Ocorreu um erro!");

toast.warning("Atenção!");

toast.info("Informação importante");
```

---

## ⚙️ Configurações Comuns

Você pode personalizar o comportamento dos toasts passando opções:

```jsx
toast.success("Salvo com sucesso!", {
  position: "top-right",
  autoClose: 3000,
  hideProgressBar: false,
  closeOnClick: true,
  pauseOnHover: true,
  draggable: true,
  theme: "colored",
});
```

### Principais opções:

|Opção|Descrição|
|---|---|
|`position`|Posição do toast na tela|
|`autoClose`|Tempo em ms para fechar automaticamente|
|`theme`|`light`, `dark` ou `colored`|
|`hideProgressBar`|Oculta a barra de progresso|
|`closeOnClick`|Fecha ao clicar|
|`pauseOnHover`|Pausa ao passar o mouse|

---

## 🎨 Posições Disponíveis

- `top-left`
- `top-right`
- `top-center`
- `bottom-left`
- `bottom-right`
- `bottom-center`

---

## 🧩 Exemplo Completo

```jsx
import { toast } from 'react-toastify';

function Button() {
  const handleClick = () => {
    toast.success("Ação executada com sucesso!");
  };

  return <button onClick={handleClick}>Mostrar Toast</button>;
}

export default Button;
```

---

## 🧪 Boas Práticas

- Use toasts para **feedback rápido**, não para mensagens longas
- Centralize mensagens comuns em um helper
- Evite excesso de notificações simultâneas
- Utilize tipos (`success`, `error`, etc.) para melhorar a UX
- Combine com validações e respostas de API

---

## 🚧 Problemas Comuns

### Toast não aparece

- Verifique se o `ToastContainer` está renderizado
- Confirme a importação do CSS
- Evite múltiplos `ToastContainer`

### Estilo quebrado

- Certifique-se de que o CSS foi importado corretamente
- Verifique conflitos com frameworks de estilo (ex: Tailwind, Bootstrap)

---

## 📚 Documentação Oficial

Para mais detalhes e exemplos avançados, consulte a documentação oficial:

👉 [https://fkhadra.github.io/react-toastify/](https://fkhadra.github.io/react-toastify/)
