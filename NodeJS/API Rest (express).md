****# Project API Rest
Manual de Projeto para API Rest, **configuração passo a passo.**

## Estruturando Project

### npm init -y
Inicialização do gerenciador de PACKAGE Node JS (package.json)

---
---
### .editorconfig
Ajuste do modo de trabalho (configurações do VSCode)

```sql

# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lt

```

---
---
### eslint / prettier (install)

#### **ESLINT**
	Instalação da ferramenta c  de análise estática de código JavaScript que serve para:
Encontrar e corrigir erros, Aplicar padrões de estilo, Evitar bugs comuns e Padronizar o código em equipe

#### **PRETTIER**
Instalação da ferramenta **Prettier** de formatação automática de código que serve para:
Formata o código de forma consistente, com base em regras de estilo predefinidas.

> _Necessário ter instalado os plugins_ <b><u>eslint / prettier.</u></b>


- **_Instale o ESLint e Prettier_**
  `npm install --save-dev eslint prettier`

- **_Instale a configuração Airbnb e dependências (regra de estilo)_**
  `npx install-peerdeps --dev eslint-config-airbnb-base`

- **_Instale plugins para ESLINT e PRETTIER trabalharem juntos_**
  `npm install --save-dev eslint-plugin-prettier eslint-config-prettier`

#### **Criando arquivo de configuração `ESLint`

> Crie o arquivo de configuração do ESLint **(.eslintrc.json)**


```json
{
  "env": {
    "node": true,
    "es2021": true
  },
  "parserOptions": {
    "ecmaVersion": 2021,
    "sourceType": "module"
  },
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "extends": ["airbnb-base", "plugin:prettier/recommended"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```


#### **Criando arquivo de configuração `Prettier`

> Crie o arquivo de configuração do Prettier **(.prettierrc)**


```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```


> Crie o arquivo de configuração do Prettier para ignorar arquivos **(.prettierignore)**
> `.prettierignore`


**OBS.** Necessário configurar o **settings.json** do VSCode incluindo essas linhas para auto-formatação do eslint e prettier<br>

```json
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.formatOnSave": true,
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": "explicit",
  "source.fixAll.all": "explicit"
}
```


---
---
### nodemon / sucrase

**Nodemon** é uma ferramenta que monitora automaticamente os arquivos do projeto e reinicia o servidor (ou script) toda vez que detecta alterações no código.

**Sucrase** é uma ferramenta de transpilaçāo rápida para JavaScript/TypeScript. Ele é uma alternativa mais leve e rápida ao Babel, focada em projetos modernos.

`npm install nodemon sucrase -D`

- Crie o arquivo de registro para **sucrase** em `nodemon.json`

```json
{
	"execMap": {
		"js": "node -r sucrase/register"
	}
}
```

---
---

## Table Students
### sequelize

**Sequelize** é um **ORM (Object-Relational Mapper)** para Node.js que facilita a comunicação com **bancos de dados relacionais** como:

- **PostgreSQL**
- **MySQL**
- **MariaDB**
- **SQLite**
- **Microsoft SQL Server**

Ele permite que você **interaja com o banco usando código JavaScript**, em vez de escrever SQL manualmente. Com Sequelize, você pode:

- Criar e manipular tabelas (models)
- Inserir, buscar, atualizar e deletar dados
- Criar e rodar migrações
- Relacionar tabelas (associações)

- **Install (sequelize)**

 `npm install sequelize mariadb`
 `npm install -D sequelize-cli`


-  **_Install (dotenv)_**
`npm install dotenv`

-  **_Create file config dotenv_** (.env)
`touch .env`

> O arquivo  **`.env`** servirá para armazenar as variáveis  de ambiente **sequelize** de acesso ao **BD**


```env
DATABASE=school
DATABASE_HOST=ip_BD
DATABASE_PORT=3306
DATABASE_USERNAME=root
DATABASE_PASSWORD=nEGO190475/*
```


-  **_Create file config for .SEQUELEIZERC_**
`touch .sequelizerc` 

>  ative **Highlight** para realçar digitação de cod. no arquivo "type JS" <span style="color:red"><b>"Opcional"</b></style> 


```js
const { resolve } = require('node:path');

module.exports = {
  config: resolve(__dirname, 'src', 'config', 'database.js'),
  'models-path': resolve(__dirname, 'src', 'models'),
  'migrations-path': resolve(__dirname, 'src', 'database', 'migrations'),
  'seeders-path': resolve(__dirname, 'src', 'database', 'seeds'),
};
```


-  **_Create file config/database.js_**
Arquivo que irá responder as configurações de acesso ao DB e ao **.sequelizerc**


```js
require('dotenv').config();

module.exports = {
  dialect: 'mariadb',
  host: process.env.DATABASE_HOST,
  port: process.env.DATABASE_PORT,
  username: process.env.DATABASE_USERNAME,
  password: process.env.DATABASE_PASSWORD,
  database: process.env.DATABASE,
  define: {
    timestamps: true,
    underscored: true,
    underscoredAll: true,
    createdAt: 'created_at',
    updatedAt: 'updated_at',
  },
  dialictOptions: {
    timezone: 'America/Sao_Paulo',
  },
  timezone: 'America/Sao_Paulo',
};
```


**Próximo passo,** realizar o acesso a **DB** via **WorkBench**  para criar um base de dados no MariaDB que se encontra dentro do servidor.

`create schema`


**Próximo passo,** criar arquivo de <span style="color: red"><b>MIGRATIONS</b></span>  (arquivos de tabelas/campos), no projeto APIRest

`npx sequelize migration:create --name=students`


**Configuração do arquivo** <span style="color: red"><b>MIGRATIONS</b></span> **criado**  `/src/database/migrations`
Campos da tabelas que será criada no **DB**  (**nome da tabela no plural**)

```js
/** @type {import('sequelize-cli').Migration} */

module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('students', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: { type: Sequelize.STRING, allowNull: false },
      lastname: { type: Sequelize.STRING, allowNull: false },
      email: { type: Sequelize.STRING, allowNull: false },
      age: { type: Sequelize.INTEGER, allowNull: false },
      weight: { type: Sequelize.FLOAT, allowNull: false },
      height: { type: Sequelize.FLOAT, allowNull: false },
      created_at: { type: Sequelize.DATE, allowNull: false },
      updated_at: { type: Sequelize.DATE, allowNull: false },
    });
  },

  async down(queryInterface) {
    await queryInterface.dropTable('students');
  },
};
// students -> referente ao module "student" no singular
```


**Próximo passo,** enviar a <span style="color: red"><b>MIGRATIONS</b></span> para o **DB** (MariaDB), para a realização da criação de tabelas/campos no **DB** <span style="color: red">(base de dados</span>).

`npx sequelize db:migrate`

> Caso precise, esse é o comando para desfazer uma **MIGRATION**

`npx sequelize db:migrate:undo`

- **Create file /src/model/Student.js**
Nome do Model no Singular iniciando c/ Maiúscula, pois é uma **Class**

> _Os **Models** são necessários para representar as **tabelas do banco de dados** e definem **como os dados serão estruturados e manipulados** dentro da aplicação._


```js
import Sequelize, { Model } from 'sequelize';

export default class Student extends Model {
  static init(sequelize) {
    super.init(
      {
        name: Sequelize.STRING,
        lastname: Sequelize.STRING,
        email: Sequelize.STRING,
        age: Sequelize.INTEGER,
        weight: Sequelize.FLOAT,
        height: Sequelize.FLOAT,
      },
      {
        sequelize,
      }
    );
    return this;
  }
}
```


-  **Create file connection file DB **  `/src/database/index.js`

> <span style="color: red"><b>OBS.</b> criando o arquivo com o nome "index.js", auxilia no hora da importação utilizando apenas o nome de pasta para importa-lo.</span>


```js
import { Sequelize } from 'sequelize';
import databaseConfig from '../config/database';
import Student from '../models/myModel';
// OBS. TODOS MODEL EXISTENTES SÃO OBGRIGATÓRIO SUA IMPORTAÇÃO.

const connection = new Sequelize(databaseConfig);
const models = [Student];

models.forEach((model) => model.init(connection));
```


> Ativando / inicializando <span style="color: red">o <b>arquivo de conexão</b></span> no **app.js**

 **OBS:** _NÃO_ é necessário especificar o arquivo na **importação**, pois o arquivo é o **index.js** da pasta, nesse caso a **importação** reconhece automaticamente o arquivo devido a nomeação **index.js**

`import ./src/database`


**Exemplo:**
Utilizando `HomeController.js`   - >  <span style="color: red"><b>GET</b></span> para criação de STUDENTS no **"Banco de Dados"**

```js
import Student from '../models/Student';

class HomeController {
  async index(req, res) {
    const newStudent = await Student.create({
      name: 'Maria ',
      lastname: 'Nascimento',
      email: 'maria@ig.com',
      age: 9,
      weight: 27,
      height: 1.38,
    });
    res.json(newStudent);
  }
}

export default new HomeController();
```

---
---

### **Resumo**

Passo a passo implantação **ORM Sequelize**

1. Instalação dos pacotes (bibliotecas/ framework):
   **`npm install sequelize mariadb` `npm install -D sequelize-cli`**
   <br>
2. Config dados de acesso **(.env)** ao banco de dados :

```env
DATABASE=school
DATABASE_HOST=ip_BD
DATABASE_PORT=3306
DATABASE_USERNAME=root
DATABASE_PASSWORD=nEGO190475/*
```
<br>
3. Criação do arquivo de configuração <span style="color: red"><b>sequelize database</b></span>

```js
require('dotenv').config();

module.exports = {
  dialect: 'mariadb',
  host: process.env.DATABASE_HOST,
  port: process.env.DATABASE_PORT,
  username: process.env.DATABASE_USERNAME,
  password: process.env.DATABASE_PASSWORD,
  database: process.env.DATABASE,
  define: {
    timestamps: true,
    underscored: true,
    underscoredAll: true,
    createdAt: 'created_at',
    updatedAt: 'updated_at',
  },
  dialictOptions: {
    timezone: 'America/Sao_Paulo',
  },
  timezone: 'America/Sao_Paulo',
};
```
<br>
4. Criação do arquivo de configuração <span style="color: red"><b>sequelize path</b>(caminhos)</span> - **_`.sequelizerc`_**

```js
const { resolve } = require('node:path');

module.exports = {
  config: resolve(__dirname, 'src', 'config', 'database.js'),
  'models-path': resolve(__dirname, 'src', 'models'),
  'migrations-path': resolve(__dirname, 'src', 'database', 'migrations'),
  'seeders-path': resolve(__dirname, 'src', 'database', 'seeds'),
};
```
<br>
5. Criação do arquivo de **MIGRATIONS** (basic) --> /src/database/migrations/**\*.js\***
   **`npx sequelize migration:create --name=students`**
   <br>
6. Personalização do arquivo de **MIGRATIONS** (tabelas students)

```js
/** @type {import('sequelize-cli').Migration} */

module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('students', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: { type: Sequelize.STRING, allowNull: false },
      lastname: { type: Sequelize.STRING, allowNull: false },
      email: { type: Sequelize.STRING, allowNull: false },
      age: { type: Sequelize.INTEGER, allowNull: false },
      weight: { type: Sequelize.FLOAT, allowNull: false },
      height: { type: Sequelize.FLOAT, allowNull: false },
      created_at: { type: Sequelize.DATE, allowNull: false },
      updated_at: { type: Sequelize.DATE, allowNull: false },
    });
  },

  async down(queryInterface) {
    await queryInterface.dropTable('students');
  },
};
```
<br>
7. Criar uma **DATABASE** no banco de dados com nome **"school"** (workbench ou via comando direto no DB)
   <br>
8. Criação **tabelas** no <span style="color: red"><b>Banco de Dados</b></span> **"school"** (migration)
   `npx sequelize db:migrate`
   <br>
9. Criação um arquivo <span style="color: red"><b>Model ''Student"</b></span> -> file /src/model/Student.js

```js
import Sequelize, { Model } from 'sequelize';

export default class Student extends Model {
  static init(sequelize) {
    super.init(
      {
        name: Sequelize.STRING,
        lastname: Sequelize.STRING,
        email: Sequelize.STRING,
        age: Sequelize.INTEGER,
        weight: Sequelize.FLOAT,
        height: Sequelize.FLOAT,
      },
      {
        sequelize,
      }
    );
    return this;
  }
}
```
<br>
10. Criação do arquivo de **CONNECTION** com o **DB** --> /src/database/index.js

```js
import { Sequelize } from 'sequelize';
import databaseConfig from '../config/database';
import Student from '../models/Student'; // MODEL
// OBS. TODOS MODEL EXISTENTS SÃO OBGRIGATÓRIO SUA IMPORTAÇÃO.

const connection = new Sequelize(databaseConfig);
const models = [Student];

models.forEach((model) => model.init(connection));
```
<br>
11. Importar arquivo de **CONNECTION** no **app.js** para inicialização da connection ao DB.
    `import ./src/database;`
<br>
12. Criação modo de create, delete, update de user para os Métodos **GET, DELETE, UPDATE**

---
---

## Table Users

**Criar arquivo** de <span style="color: red"><b>MIGRATIONS</b></span>  (arquivos de tabelas/campos) for Users

`npx sequelize migration:create --name=users`

---

**Configuração do arquivo** <span style="color: red"><b>MIGRATIONS</b></span> **criado**  `/src/database/migrations`
Campos da tabelas que será criada no **DB**  (**nome da tabela no plural**)

```js
/** @type {import('sequelize-cli').Migration} */

module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('users', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: { type: Sequelize.STRING, allowNull: false },
      email: { type: Sequelize.STRING, allowNull: false, unique: true },
      password_hash: { type: Sequelize.STRING, allowNull: false },
      created_at: { type: Sequelize.DATE, allowNull: false },
      updated_at: { type: Sequelize.DATE, allowNull: false },
    });
  },

  async down(queryInterface) {
    await queryInterface.dropTable('users');
  },
};
// users -> referente ao module "user" no singular
```
<br>

**Próximo passo,** enviar a <span style="color: red"><b>MIGRATIONS</b></span> para o **DB** (MariaDB), para a realização da criação de tabelas/campos no **DB** <span style="color: red">(base de dados</span>).

`npx sequelize db:migrate`

> Caso precise, esse é o comando para desfazer uma **MIGRATION**

`npx sequelize db:migrate:undo`

---

- **Create file /src/model/User.js** <span style="color: red"><b>(com validação)</b></span>
Nome do Model no Singular iniciando c/ Maiúscula, pois é uma **Class**.

> _Os **Models** são necessários para representar as **tabelas do banco de dados** e definem **como os dados serão estruturados e manipulados** dentro da aplicação, os campos são validados utilizando as biblioteca **`bcryptjs`** 

Instalar a biblioteca **_`bcryptJS`_** para a realização de validação do campo informados.

`npm install bcryptjs`


```js
import Sequelize, { Model } from 'sequelize';
import bcrypt from 'bcryptjs';

class User extends Model {
	static init(sequelize) {
		super.init(
			{
				name: {
					type: Sequelize.STRING,
					defaultValue: '',
					validate: {
						len: {
							args: [6, 50],
							msg: 'The password must contain between 6 and 50 characters.',
						},
					},
				},
				email: {
					type: Sequelize.STRING,
					defaultValue: '',
					unique: {
						msg: 'Error, email já existe!',
					},
					validate: {
						isEmail: {
							msg: 'Email invalid!',
						},
					},
				},
				password_hash: {
					type: Sequelize.STRING,
					defaultValue: '',
				},
				password: {
					type: Sequelize.VIRTUAL,
					defaultValue: '',
					validate: {
						len: {
							args: [3, 25],
							msg: 'The password must contain between 3 and 25 characters',
						},
					},
				},
			},{ 
			sequelize,
			});
			
		this.addHook('beforeSave', async (user) => {
			if(user.password) {
				user.password_hash = await bcrypt.hash(user.password, 8);
			}
		});
		
		return this;
	}
}

```
<br>
#### O que é Hooks no Sequelize

Hooks no **Sequelize** / ou **lifecycle hooks** são funções que o **Sequelize** executa automaticamente em determinados **momentos do ciclo de vida de um modelo**.
Exemplo:

- antes de criar (`beforeCreate`)
- depois de criar (`afterCreate`)
- antes de atualizar (`beforeUpdate`)
- antes de deletar (`beforeDestroy`)
- etc.<br>
> Eles servem para **executar código automaticamente** quando algo acontece no banco — como **criptografar uma senha antes de salvar**, **gerar logs**, **validar dados**, etc.


#### Entendimento da validação

**Campos Tabela:**

- **`name`**
	migrado no banco de dados campo como: `type: string` e `allowNull: false` (sendo obrigatório sua declaração).
	
	modelo de validação como:  `type: string`, `defaultValue: " "`, `validate: { leng:{ args: [6, 50]` (tamanho), `msg: "Personalizada"`}}     

- **`email`** 
	migrado no banco de dados campo como: `type: string` e `allowNull: false` (sendo obrigatório sua declaração), `unique: true` (definição como único).
	
	modelo de validação como: `type: string`, `defaultValue: " "`, `unique: { msg: "Personalizada" }`, `validate: { isEmail: { msg: "Personalizada" }}

 - **`password`** _não existe no DB, enviado via HOOK no campo **`password_hash`**
	 migrado no banco de dados campo como **`password_hash`**: `type: string` e `allowNull: false` (sendo obrigatório sua declaração).
	 
	 **`password_hash`** -> modelo de validação campo como : `type: string` e `allowNull: false` (sendo obrigatório sua declaração).
	 
	 **`password`** -> modelo de validação campo como: `type: virtual`, `defaultValue: " "`, `validate: { len: { args: [3, 25], msg: "Personalizada"`}}

A migração (migration) é onde criamos os campo da tabela no Banco de dados, informando como as campos vão funcionar.

Os Models e onde definimos como as estruturas dos dados e suas validações iram funcionar.

E os Controllers são como iram receber ou obter os dados do Banco de dados.
<br>
### Todos Models criados é necessário seu _`import`_ na database (index)

-  **Import Model `user.js` in  `/src/database/index.js`

```js
import User from "../models/User"
// OBS. TODOS MODEL EXISTENTES SÃO OBGRIGATÓRIO SUA IMPORTAÇÃO.

const connection = new Sequelize(databaseConfig);
const models = [Student, User];

models.forEach((model) => model.init(connection));
```
<br>

**Exemplo:**
Utilizando `UserController.js`   - >  <span style="color: red"><b>POST</b></span> para criação de STUDENTS no **"Banco de Dados"**

```js
import User from '../models/User';

class UserController {
  async store(req, res) {
    const newUser = await User.create({
      name: 'Joaquim',
      email: 'joka@ig.com',
      password: 123456,
    });
    res.json(newUser);
  }
}

export default new UserController();
```

> OBS:. Não esquecer de criar Rotas ...
> 
---
---
<br>
## Methods (rotes/controllers)

JWT token

1. Criação do Token / time expired (.env)
	`TOKEN_SECURE=qwd+586er-safE/RT*WGDS6-5g4-regsf!gsr-e24t5rTBd@tVESWD`
	`TOKEN_EXPIRED=7d

2. Install JWT Web Token
	`npm i jsonwebtoken`

3. Criação do Controller Token


## Configuração SEQUELIZE

1. install `dotenv`
2. inserir dados do acesso ao db no `.env`
3. import o `dotenv` para inicialização no project. 
	1. `import dotenv from 'dotenv';
	2. `dotenv.config();`
4. configuração `.sequelizerc` ("Object" de configuração para estrutura do **sequelize**)
5. configuração do arquivo com os dados de connectionDB do **sequelize**(`config/database.js`).
6. configuração da database (Servidor - criação do BD, **Workbench**).
7. criação da tabelas na database (Servidor)
	1. `npx sequelize migration:create --name=nameTable`
8.  configuração das regras para os campos da tabela (arquivo de **`migration.js`**)
	1. `/src/database/migration/*.js`
9. Inserindo configurações da **migration** na tabela in database (Servidor)
	1. `npx sequelize db:migrated`
10. criação e configuração do arquivo de **Model**.
	1. Os **Models** definem na aplicação como os dados serão estruturados, manipulados e validados na **database(DB)**.
	2. `/src/model/*.js
11.  criação e configuração do arquivo de importação de dados (**Models**) para database(Servidor)
	1. `/src/database/index.js` 
12. inicializar arquivo de importação no arquivo _principal da app_ (**database/index.js**).
	1. `import './src/database`'
13. create controller para inserção dos dados no **SERVIDOR** (find, create, update, delete)


## JWT

**JSON Web Token**, e é um **padrão aberto** (RFC 7519) usado para **autenticação e troca segura de informações** entre sistemas — geralmente entre o **cliente (front-end)** e o **servidor (back-end)**.

### Token

Um **token** é basicamente um **código de acesso** que representa a identidade de um usuário autenticado.  
Em vez de o usuário enviar login e senha a cada requisição, ele envia esse token — que contém informações codificadas e assinadas digitalmente.


## Seeds

Em **programação**, especialmente quando trabalhamos com **bancos de dados** (como no **Sequelize**, **TypeORM**, **Prisma**, etc.), o termo **“seeds”** (ou “seeders”) significa **dados iniciais** ou **dados de preenchimento**.

### **Significado literal**

“Seed” em inglês quer dizer **semente**.  
A ideia é que você “planta” dados iniciais no banco — como sementes — para que o sistema comece funcionando com alguma informação básica.

### **Na prática**

Um **arquivo de seed** é um script que **insere registros automáticos no banco** de dados.  
Você o executa normalmente depois de criar ou migrar as tabelas.

Por exemplo, no **Sequelize**, você pode ter algo como:

```bash
npx sequelize-cli db:seed:all
```

Isso executa todos os arquivos de seeds.

---

### **Exemplo de uso no Sequelize**

**Arquivo:** `20251113-demo-user.js`

```js
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.bulkInsert('Users', [
      {
        name: 'Admin',
        email: 'admin@example.com',
        password_hash: '123456',
        created_at: new Date(),
        updated_at: new Date(),
      },
      {
        name: 'Guest',
        email: 'guest@example.com',
        password_hash: '654321',
        created_at: new Date(),
        updated_at: new Date(),
      },
    ]);
  },

  async down(queryInterface, Sequelize) {
    await queryInterface.bulkDelete('Users', null, {});
  },
};
```

---

### **Quando usar seeds**

Você usa **seeds** quando quer:

- Criar **usuários padrão** (ex: admin);
- Popular o banco com **dados de teste**;
- Configurar **valores iniciais** (categorias, permissões, etc.);
- Garantir que o sistema funcione logo após a instalação.


### **Diferença entre “migrations” e “seeds”**

| Conceito      | O que faz                                                                 |
| ------------- | ------------------------------------------------------------------------- |
| **Migration** | Cria ou altera a **estrutura** do banco (tabelas, colunas, índices, etc.) |
| **Seed**      | Insere **dados** no banco (registros iniciais ou de teste)                |

---

<h3><big><b>Passo a passo completo pra criar e rodar seeds no Sequelize CLI</b></big></h3>
#### **Pré-requisitos**

Você precisa ter o **Sequelize CLI** instalado no seu projeto e o banco configurado.

```bash
npm install --save sequelize sequelize-cli
```

---

> **Se ainda não fez a configuração inicial:**

```bash
npx sequelize-cli init
```

Isso cria a estrutura:

```
/models
/migrations
/seeders
/config/config.json
```

---

###  **1. Criar um arquivo de seed**

Você pode criar um seed manualmente **ou via CLI**:

```bash

npx sequelize-cli seed:generate --name demo-user # recomendado atualmente
npx sequelize-cli seed:create --name demo-user # legado (old)

```

> **OBS:** Nas versões mais antigas do **`sequelize-cli`**, usava-se **`seed:create`**, agora  o time do Sequelize **padronizou tudo com o formato `seed:generate`**, então agora esse é o recomendado.
> Os dois command fazem **exatamente a mesma coisa**. (em versões mais antiga apenas muda o command)

<br>
Isso cria um arquivo em `/seeders`, algo como:

```
20251113-demo-user.js
```


---

### **2. Editar o seed**

Abra o arquivo criado e adicione o conteúdo:

```js
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.bulkInsert('Users', [
      {
        name: 'Admin',
        email: 'admin@example.com',
        password_hash: '123456',
        created_at: new Date(),
        updated_at: new Date(),
      },
      {
        name: 'Guest',
        email: 'guest@example.com',
        password_hash: '654321',
        created_at: new Date(),
        updated_at: new Date(),
      },
    ]);
  },

  async down(queryInterface, Sequelize) {
    await queryInterface.bulkDelete('Users', null, {});
  },
};
```

---

### **3. Rodar os seeds**

Depois que o arquivo estiver pronto, execute:

```bash
npx sequelize-cli db:seed:all
```

👉 Isso roda **todos os seeds** do diretório `/seeders`.

Se quiser rodar apenas um seed específico:

```bash
npx sequelize-cli db:seed --seed 20251113-demo-user.js
```

---

### **4. Desfazer os seeds (opcional)**

Para apagar os dados inseridos:

```bash
npx sequelize-cli db:seed:undo:all
```

Ou apenas um específico:

```bash
npx sequelize-cli db:seed:undo --seed 20251113-demo-user.js
```

---

### **Resumo**

|Comando|O que faz|
|---|---|
|`npx sequelize-cli seed:generate --name nome`|Cria um novo arquivo de seed|
|`npx sequelize-cli db:seed:all`|Executa todos os seeds|
|`npx sequelize-cli db:seed --seed nome.js`|Executa um seed específico|
|`npx sequelize-cli db:seed:undo:all`|Reverte todos os seeds|
|`npx sequelize-cli db:seed:undo --seed nome.js`|Reverte um seed específico|

---

### **Passo a passo** como criar um _seed_ dinâmico com usuários falsos.
Forma prática de gerar dados realistas automaticamente para testes.


#### Instalar o Faker

No seu projeto Node, rode:

```bash
npm install @faker-js/faker
```

---

#### Gerar o arquivo de seed

Crie um novo _seed_ (por exemplo, `fake-users`):

```bash
npx sequelize-cli seed:generate --name fake-users
```

Isso cria algo como:

```
/seeders/20251113123000-fake-users.js
```

---

#### Editar o seed para gerar dados automáticos

Abra o arquivo e substitua por este conteúdo:

```js
'use strict';

import { faker } from '@faker-js/faker';

export default {
  async up (queryInterface, Sequelize) {
    const fakeUsers = [];

    for (let i = 0; i < 10; i++) {
      fakeUsers.push({
        name: faker.person.fullName(),
        email: faker.internet.email(),
        password_hash: faker.internet.password(),
        created_at: new Date(),
        updated_at: new Date(),
      });
    }

    await queryInterface.bulkInsert('Users', fakeUsers, {});
  },

  async down (queryInterface, Sequelize) {
    await queryInterface.bulkDelete('Users', null, {});
  }
};
```

---

#### Executar o seed

Rode:

```bash
npx sequelize-cli db:seed:all
```

🔹 Isso vai inserir **10 usuários falsos** automaticamente na tabela `Users`.  
Cada vez que você executar o seed, novos nomes e e-mails serão gerados.

---

#### 💡 Dicas extras

- Se quiser gerar mais usuários, basta mudar `for (let i = 0; i < 10; i++)` para outro número.
    
- Você pode adaptar para outras tabelas (por exemplo, `Products`, `Students`, `Orders`, etc.) apenas trocando o nome da tabela e os campos.
    
- O `faker` tem muitas funções legais, como:
    
    ```js
    faker.location.city();         // cidade
    faker.internet.userName();     // nome de usuário
    faker.phone.number();          // número de telefone
    faker.image.avatar();          // URL de avatar
    faker.date.past();             // data antiga
    ```
    

---

### Função `down()` dentro de qualquer _seed_ (ou _migration_) do Sequelize.

#### Explicação e detalhes 


#### O que é a função `down()`

No Sequelize, **todas as seeds e migrations** têm duas partes:

| Método   | O que faz                                              |
| -------- | ------------------------------------------------------ |
| `up()`   | Executa a ação — insere, cria ou altera algo no banco. |
| `down()` | Desfaz a ação feita pelo `up()` (reverte).             |

---

#### No caso de _seeders_

Em seeds, o `up()` normalmente **insere dados**, e o `down()` serve para **removê-los**, caso você queira “desfazer” o seed.

Exemplo:

```js
async up (queryInterface, Sequelize) {
  await queryInterface.bulkInsert('Users', [
    { name: 'John', email: 'john@example.com' },
    { name: 'Jane', email: 'jane@example.com' }
  ]);
},
```

👉 Isso adiciona dois usuários à tabela `Users`.

Já o `down()`:

```js
async down (queryInterface, Sequelize) {
  await queryInterface.bulkDelete('Users', null, {});
}
```

👉 **Remove** os registros inseridos (ou todos os registros da tabela, dependendo dos parâmetros).

---

#### Quando o `down()` é usado

Você executa ele **quando quer desfazer seeds** com o comando:

```bash
npx sequelize-cli db:seed:undo:all
```

ou

```bash
npx sequelize-cli db:seed:undo --seed nome-do-arquivo.js
```

⚙️ Assim, o Sequelize executa automaticamente o método `down()` de cada _seed_, revertendo o que o `up()` fez.

---

#### O que significam os parâmetros do `bulkDelete`

```js
await queryInterface.bulkDelete('Users', null, {});
```

|Parâmetro|Significado|
|---|---|
|`'Users'`|Nome da tabela que será afetada.|
|`null`|Filtro `WHERE`. `null` significa “sem filtro”, ou seja, apaga **tudo**.|
|`{}`|Opções extras (normalmente deixadas vazias).|

---

### Em resumo

📌 **Função `up()`** → insere dados.  
📌 **Função `down()`** → apaga os dados inseridos (reversão).

Ela serve para que você possa **voltar atrás** caso rode seeds de teste e queira limpar o banco.

---

### Deletar **somente os registros inseridos por aquele seed** (sem apagar a tabela toda).
Evitar apagar dados reais ou de outros seeds.

---

#### Situação atual (apaga tudo)

O exemplo padrão é:

```js
await queryInterface.bulkDelete('Users', null, {});
```

🧨 Isso **remove todos os registros da tabela `Users`**, sem exceção — o que é perigoso se você tiver dados reais.

---

#### Limitando o delete com `where`

Você pode passar um **filtro `where`** no segundo parâmetro, igual ao `WHERE` do SQL.

Por exemplo, se seus usuários de teste têm e-mails que terminam com `@example.com`:

```js
await queryInterface.bulkDelete('Users', {
  email: ['john@example.com', 'jane@example.com']
}, {});
```

👉 Isso apaga **apenas** os registros com esses e-mails.

---

#### Usando uma condição mais genérica

Se seus usuários “fakes” seguem um padrão (por exemplo, e-mails terminando em `@fake.com`), você pode usar o operador `Op.like`:

```js
const { Op } = Sequelize;

await queryInterface.bulkDelete('Users', {
  email: { [Op.like]: '%@fake.com' }
}, {});
```

👉 Isso apaga **somente usuários de teste**.

---

#### Exemplo completo (revisado)

Aqui está um seed completo e seguro 👇

```js
'use strict';

import { faker } from '@faker-js/faker';

export default {
  async up (queryInterface, Sequelize) {
    const fakeUsers = [];

    for (let i = 0; i < 10; i++) {
      fakeUsers.push({
        name: faker.person.fullName(),
        email: faker.internet.email({ provider: 'fake.com' }),
        password_hash: faker.internet.password(),
        created_at: new Date(),
        updated_at: new Date(),
      });
    }

    await queryInterface.bulkInsert('Users', fakeUsers, {});
  },

  async down (queryInterface, Sequelize) {
    const { Op } = Sequelize;
    await queryInterface.bulkDelete('Users', {
      email: { [Op.like]: '%@fake.com' },
    }, {});
  }
};
```

✅ **Resultado:**

- O `up()` insere 10 usuários com e-mails `@fake.com`.
- O `down()` remove apenas esses usuários, **sem tocar nos reais**.

---

#### 🧠 Dica avançada

Você também pode:

- Criar um campo extra no seed (ex: `is_fake: true`) e filtrar por ele no `down()`;
- Ou registrar IDs inseridos em um array e depois deletá-los diretamente:```js

```

await queryInterface.bulkDelete('Users', { id: idsInseridos }, {});

```


---

### **Método mais preciso e seguro** para reverter _seeds_,
Apaga **exatamente** os registros criados pelo `up()` — sem depender de padrões de e-mail ou filtros genéricos.

#### A ideia

Quando você insere registros com `queryInterface.bulkInsert()`, ele **não retorna os IDs inseridos** automaticamente.  
Mas você pode:

- Inserir os dados primeiro,
    
- Depois buscar os IDs desses registros com um `SELECT` (usando `queryInterface.sequelize.query`).
    

Assim, o `down()` pode usar esses mesmos IDs para excluir apenas o que foi criado.

#### Exemplo completo com Sequelize + Faker

```js
'use strict';

import { faker } from '@faker-js/faker';

export default {
  async up (queryInterface, Sequelize) {
    const fakeUsers = [];

    for (let i = 0; i < 5; i++) {
      fakeUsers.push({
        name: faker.person.fullName(),
        email: faker.internet.email({ provider: 'fake.com' }),
        password_hash: faker.internet.password(),
        created_at: new Date(),
        updated_at: new Date(),
      });
    }

    // Inserindo usuários
    await queryInterface.bulkInsert('Users', fakeUsers, {});
  },

  async down (queryInterface, Sequelize) {
    // Buscar IDs dos usuários criados no seed
    const [results] = await queryInterface.sequelize.query(`
      SELECT id FROM "Users"
      WHERE email LIKE '%@fake.com';
    `);

    // Extrair IDs em um array
    const ids = results.map(u => u.id);

    // Se encontrou algum, apagar só esses
    if (ids.length > 0) {
      await queryInterface.bulkDelete('Users', { id: ids }, {});
    }
  }
};
```


#### Entendendo o que acontece

| Etapa                              | O que faz                                               |
| ---------------------------------- | ------------------------------------------------------- |
| `bulkInsert`                       | Insere vários registros no banco.                       |
| `sequelize.query()`                | Executa uma query SQL manualmente.                      |
| `map(u => u.id)`                   | Cria uma lista só com os IDs dos registros encontrados. |
| `bulkDelete('Users', { id: ids })` | Apaga apenas os registros que têm esses IDs.            |

---

#### Resultado

- O `up()` insere 5 usuários fakes.
- O `down()` busca os IDs de todos os usuários com e-mails `@fake.com` e **remove só eles**.
- Nenhum outro dado da tabela é afetado.


💡 **Dica extra:**  
Se quiser mais controle, você pode salvar os dados gerados num arquivo `.json` ou numa tabela auxiliar (por exemplo, `SeedLog`) e depois usá-los no `down()`.

---
	