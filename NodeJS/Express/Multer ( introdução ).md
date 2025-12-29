## [[#^079559|Multer in API]]

# 📦 Tutorial Completo de Multer (Node.js + Express)

Este tutorial explica **em detalhes todas as formas de usar o Multer**, desde o básico até configurações avançadas e implantação em produção.

---

## 📌 O que é o Multer?

**Multer** é um middleware para **Node.js** usado com **Express** para lidar com uploads de arquivos via formulários `multipart/form-data`.

Ele permite:

- Receber arquivos no back-end
- Salvar arquivos em disco ou memória
- Validar tipo e tamanho
- Controlar nomes e pastas
- Acessar arquivos via `req.file` ou `req.files`
    

---

## 🧱 Pré-requisitos

- Node.js instalado
- Conhecimento básico de Express
- Projeto inicializado com `npm init`

---

## 📥 Instalação

```bash
npm install multer
```

---

## 🚀 Configuração Básica

### Estrutura inicial

```
project/
 ├─ uploads/
 ├─ src/
 │   ├─ app.js
 │   └─ routes.js
```

### Uso simples

```js
const multer = require('multer');
const upload = multer({ dest: 'uploads/' });
```

Esse modo:

- Salva arquivos automaticamente
- Gera nomes aleatórios
- Não valida tipo ou tamanho

---

## 📤 Upload de Arquivo Único

```js
app.post('/upload', upload.single('arquivo'), (req, res) => {
  console.log(req.file);
  res.send('Upload realizado');
});
```

- `single()` → apenas 1 arquivo
- Campo do formulário: `name="arquivo"`

📌 Dados disponíveis em `req.file`

---

## 📤 Upload de Múltiplos Arquivos

```js
app.post('/uploads', upload.array('arquivos', 5), (req, res) => {
  console.log(req.files);
  res.send('Arquivos enviados');
});
```

- Máximo definido pelo segundo parâmetro
- Dados disponíveis em `req.files`

---

## 📤 Upload com Campos Diferentes

```js
upload.fields([
  { name: 'foto', maxCount: 1 },
  { name: 'documentos', maxCount: 3 }
]);
```

Retorno:

```js
req.files.foto
req.files.documentos
```

---

## 🧠 Armazenamento em Disco (DiskStorage)

```js
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const ext = file.originalname.split('.').pop();
    cb(null, `${Date.now()}.${ext}`);
  }
});

const upload = multer({ storage });
```

✔ Controle total do nome e local

---

## 🧠 Armazenamento em Memória

```js
const upload = multer({ storage: multer.memoryStorage() });
```

📌 Útil para:

- Enviar para S3, Firebase, Cloudinary
- Não salvar arquivos localmente

⚠ Não recomendado para arquivos grandes

---

## 🔒 Validação de Tipo de Arquivo

```js
const fileFilter = (req, file, cb) => {
  if (file.mimetype.startsWith('image/')) {
    cb(null, true);
  } else {
    cb(new Error('Tipo não permitido'), false);
  }
};
```

---

## 📏 Limite de Tamanho

```js
const upload = multer({
  limits: { fileSize: 2 * 1024 * 1024 }
});
```

➡ Limite de 2MB

---

## ⚠ Tratamento de Erros

```js
app.post('/upload', (req, res) => {
  upload.single('arquivo')(req, res, err => {
    if (err) {
      return res.status(400).json({ error: err.message });
    }
    res.send('Sucesso');
  });
});
```

---

## 📂 Tornar Arquivos Públicos

```js
app.use('/uploads', express.static('uploads'));
```

Acesso via navegador:

```
http://localhost:3000/uploads/arquivo.jpg
```

---

## ☁ Integração com Serviços de Nuvem

Fluxo comum:

1. `memoryStorage()`
2. Enviar buffer para serviço externo
3. Não salvar localmente

Exemplos de serviços:

- AWS S3
- Cloudinary
- Firebase Storage

---

## 🛡 Boas Práticas

✔ Validar tipo e tamanho  
✔ Não confiar no nome original  
✔ Criar pastas por contexto  
✔ Proteger rotas com autenticação  
✔ Limpar arquivos não usados

---

## 🚀 Implantação (Deploy)

### Atenção em produção

- Plataformas como **Vercel** e **Netlify** NÃO mantêm arquivos locais
- Use armazenamento externo

### Opções

- VPS (DigitalOcean, EC2)
- AWS S3
- Cloudinary
- Firebase Storage
    
---

## 📌 Resumo

|Recurso|Multer|
|---|---|
|Upload de arquivos|✅|
|Validação|✅|
|Disco|✅|
|Memória|✅|
|Cloud|✅|

---

# 🚀 API REST Completa com Multer (Projeto Real)

^079559

Este guia mostra **uma API REST real e funcional**, usando **Node.js + Express + Multer**, com upload de arquivos, autenticação JWT e boas práticas de produção.

---

## 📁 Estrutura do Projeto

```
api-upload/
 ├─ uploads/
 │   └─ images/
 ├─ src/
 │   ├─ config/
 │   │   ├─ multer.js
 │   │   └─ auth.js
 │   ├─ controllers/
 │   │   └─ UserController.js
 │   ├─ middlewares/
 │   │   └─ authMiddleware.js
 │   ├─ routes/
 │   │   └─ user.routes.js
 │   ├─ app.js
 │   └─ server.js
 ├─ .env
 └─ package.json
```

---

## 📦 Dependências

```bash
npm install express multer jsonwebtoken bcrypt dotenv
```

```bash
npm install -D nodemon
```

---

## ⚙ Configuração do Servidor

### `src/app.js`

```js
const express = require('express');
const routes = require('./routes/user.routes');

const app = express();
app.use(express.json());
app.use('/uploads', express.static('uploads'));
app.use(routes);

module.exports = app;
```

---

### `src/server.js`

```js
require('dotenv').config();
const app = require('./app');

app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});
```

---

## 📂 Configuração do Multer

### `src/config/multer.js`

```js
const multer = require('multer');
const path = require('path');

module.exports = {
  storage: multer.diskStorage({
    destination: (req, file, cb) => {
      cb(null, 'uploads/images');
    },
    filename: (req, file, cb) => {
      const ext = path.extname(file.originalname);
      cb(null, `${Date.now()}${ext}`);
    }
  }),
  limits: {
    fileSize: 2 * 1024 * 1024
  },
  fileFilter: (req, file, cb) => {
    if (file.mimetype.startsWith('image/')) {
      cb(null, true);
    } else {
      cb(new Error('Apenas imagens são permitidas'));
    }
  }
};
```

---

## 🔐 Autenticação JWT

### `src/config/auth.js`

```js
module.exports = {
  secret: 'chave-super-secreta',
  expiresIn: '1d'
};
```

---

### `src/middlewares/authMiddleware.js`

```js
const jwt = require('jsonwebtoken');
const authConfig = require('../config/auth');

module.exports = (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader) {
    return res.status(401).json({ error: 'Token não fornecido' });
  }

  const [, token] = authHeader.split(' ');

  try {
    const decoded = jwt.verify(token, authConfig.secret);
    req.userId = decoded.id;
    return next();
  } catch {
    return res.status(401).json({ error: 'Token inválido' });
  }
};
```

---

## 👤 Controller (Upload Real)

### `src/controllers/UserController.js`

```js
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const authConfig = require('../config/auth');

const users = [];

class UserController {
  async register(req, res) {
    const { email, password } = req.body;
    const hashed = await bcrypt.hash(password, 8);

    users.push({ id: users.length + 1, email, password: hashed });
    return res.status(201).json({ message: 'Usuário criado' });
  }

  async login(req, res) {
    const { email, password } = req.body;
    const user = users.find(u => u.email === email);

    if (!user || !(await bcrypt.compare(password, user.password))) {
      return res.status(401).json({ error: 'Credenciais inválidas' });
    }

    const token = jwt.sign({ id: user.id }, authConfig.secret, {
      expiresIn: authConfig.expiresIn
    });

    return res.json({ token });
  }

  async uploadAvatar(req, res) {
    return res.json({
      userId: req.userId,
      file: req.file
    });
  }
}

module.exports = new UserController();
```

---

## 🛣 Rotas

### `src/routes/user.routes.js`

```js
const express = require('express');
const multer = require('multer');
const multerConfig = require('../config/multer');
const authMiddleware = require('../middlewares/authMiddleware');
const UserController = require('../controllers/UserController');

const routes = express.Router();
const upload = multer(multerConfig);

routes.post('/register', UserController.register);
routes.post('/login', UserController.login);
routes.post('/users/avatar', authMiddleware, upload.single('avatar'), UserController.uploadAvatar);

module.exports = routes;
```

---

## 🧪 Testando a API

### Registrar usuário

```
POST /register
```

### Login

```
POST /login
```

### Upload protegido

```
POST /users/avatar
Authorization: Bearer TOKEN
Form-data: avatar (file)
```

---
---
---

##  ARMAZENAR IMAGEM COM 'MULTER COMPLETO' - (UDEMY)

Armazenamento de image no Banco de dados - **Integração**
<br>
_**Passo a passo**_:
Armazenamento das imagens no **banco da dados** ( relações entre tabelas ).

### 1. Install Multer

```bash
npm install multer
```
<br>

---

### 2. Config Multer file - (arquivo de configuração)

```js
import multer from 'multer';
import { extname, resolve } from 'node:path';

const random = () => Math.floor(Math.random() * 10000 + 10000);

export default {
	
	// Function que filtra o tipo de arquivo (PNG/JPG)
	fileFilter: (req, file, cb) => {
		if (file.mimetype !== 'image/png' && file.mimetype !== 'image/jpeg')
			return cb(
				new multer.MulterError('The file must be in PNG or JPEG format.')
			);
		return cb(null, true);
	},
	
	// Function que define o lugar e nome do arquivo(image) a armazenar.
	storage: multer.diskStorage({
		destination: (req, file, cb) => {
			cb(null, resolve(__dirname, '..', '..', 'uploads'));
		},
		filename: (req, file, cb) => {
			cb(null, `${Date.now()}_${random()}${extname(file.originalname)}`);
		},
	}),
};
```
<br>

---

## Integração
### 3. Create migration (tabela)

```bash
npx sequelize migration:generate --name photos_table
```
<br>

---

### 4. Configuration Migration file

```js
/** @type {import('sequelize-cli').Migration} */

module.exports = {

	async up(queryInterface, Sequelize) {
		await queryInterface.createTable('photos', {
			id: {
				type: Sequelize.INTEGER,
				autoIncrement: true,
				primaryKey: true,
				allowNull: false,
			},
			originalname: {
				type: Sequelize.STRING,
				allowNull: false,
			},
			filename: {
				type: Sequelize.STRING,
				allowNull: false,
			},
			student_id: {
				type: Sequelize.INTEGER,
				allowNull: true,
 				references: {
				model: 'students',
				key: 'id',
				},
				onDelete: 'SET NULL',
				onUpdate: 'CASCADE',
			},
			created_at: {
				type: Sequelize.DATE,
				allowNull: false,
			},
			updated_at: {
				type: Sequelize.DATE,
				allowNull: false,
			},
		});
	},
	
	async down(queryInterface) {
		await queryInterface.dropTable('photos');
	},

};
```
<br>

---

### 5. Create tabela in DB (migrate)

```bash
npx db:migrate
```
<br>

---

### 6. Create Model (photo)

```js
import Sequelize, { Model } from 'sequelize';

class PhotoModel extends Model {
	static init(sequelize) {
		super.init(
			{
				originalname: {
					type: Sequelize.STRING,
					defaultValue: '',
					validate: {
						notEmpty: {
						msg: 'The Originalname field cannot be empty.',
						},
					},
				},
				filename: {
					type: Sequelize.STRING,
					defaultValue: '',
					validate: {
						notEmpty: {
							msg: 'The Filename field cannot be empty.',
						},
					},
				},
			},
			{ sequelize, tableName: 'photos' }
		);
	
		return this;
	}
	
	// Function para relação entre table (1:1) 
	static associate(models) {
		this.belongsTo(models.StudentModel, { foreignKey: 'student_id' });
	}
}

export default PhotoModel;
```

---

### 7. Include Model - connection (`index.js`)

```js
import { Sequelize } from 'sequelize';
import databaseConfig from '../config/database';
import Student from '../models/StudentModel';
import User from '../models/UserModel';
import Photo from '../models/PhotoModel';

const connection = new Sequelize(databaseConfig);
const models = [Student, User, Photo];

models.forEach((model) => model.init(connection));
models.forEach(
	(model) => model.associate && model.associate(connection.models)
);
```
<br>

---

### 8. Create Controller (photo)

```js
import multer from 'multer';
import multerConfig from '../config/multerConfig';
import PhotoModel from '../models/PhotoModel';

const upload = multer(multerConfig).single('photo');

class PhotoControllers {
	store(req, res) {
		return upload(req, res, async (error) => {
			if (error)
				return res.status(400).json({
					errors: [error.code],
			});

			try {
				const { originalname, filename } = req.file;
				const { student_id } = req.body;
			
				const photo = await PhotoModel.create({
					originalname,
					filename,
					student_id,
				});
		
				return res.json(photo);
		
			} catch (e) {
				return res.status(400).json({
					errors: 'Student not found!',
				});
			}
	
		});
	}

}

export default new PhotoControllers();
```
<br>
## 8.1 Ajuste na visualização - Controller `student.js` 

Métodos **`index`** and **`show`**

```js

class StudentControllers {

	async index(req, res) {
		try {
		// findall, especificando atributo a serem retorandos.
			const students = await Student.findAll({
				attributes: ['name', 'lastname', 'email', 'classname'],
				order: [['id', 'DESC'],	[PhotoModel, 'id', 'DESC']],
				include: {
					model: PhotoModel,
					attributes: ['url', 'originalname', 'filename'],
				},
			});
			
			if (students.length === 0) {
				return res.status(200).json({
					students: [],
					msg: 'There is no data in the database.',
				});
			}
			
			return res.status(200).json(students);
		} catch (e) {
			return res.status(500).json({
				errors: ['Internal server error.'],
			});
		}
	}
	
	async show(req, res) {
		const id = Number(req.params.id);
		
		if (!id || !Number.isInteger(id))
			return res.status(400).json({
				msg: 'Invalid id',
			});
		
		try {
			const student = await Student.findByPk(id, {
				attributes: ['name', 'lastname', 'email', 'classname'],
				order: [['id', 'DESC'],	[PhotoModel, 'id', 'DESC']],
				include: {
					model: PhotoModel,
					attributes: ['url', 'originalname', 'filename'],
				},
			});
		
			if (!student)
				return res.status(404).json({
					student: null,
					msg: 'Student not found',
				});
			return res.status(200).json(student);
		} catch (e) {
			return res.status(500).json({
				errors: e.errors ? e.errors.map((err) => err.message) : [e.message],
			});
		}
	}
	
}

export default new StudentControllers();
```
<br>

---

## 9. Create path `static` (uploads. image)

 - Create file **`src/config/appConfig.js`**

```js
// src/config/appConfig.js

export default {
	url: `${process.env.URL_PATH}:3001`,
};
```
<br>
- Configura **`modelPhoto`** (attribute VIRTUAL)

```js
// src/models/PhotoModel.js

super.init(
	{
		url: {
			type: Sequelize.VIRTUAL,
			get() {
				return`${appConfig.url}/images/${this.getDataValue('filename')}`;
			},
		},
	},
);
```
<br>
-  Configura **`statis`** in `src/app.js`

```js
// src/app.js

import { resolve } from 'node:path';

express.static(resolve(__dirname, '..', 'uploads'));
```


- Confirme a configuração do _destination in **`MULTER`** 

```js
storage: multer.diskStorage({
	destination: (req, file, cb) => {
		cb(null, resolve(__dirname, '..', '..', 'uploads', 'images'));
	},
	filename: (req, file, cb) => {
		cb(null, `${Date.now()}_${random()}${extname(file.originalname)}`);
	},
}),

```
<br> 

- Ativar a **URL** no return no **`contrutor`** desejado.

```js
class StudentControllers {
	
	async index(req, res) {
	
		try {
		// findall, especificando atributo a serem retorandos.
		const students = await Student.findAll({
			attributes: ['name', 'lastname', 'email', 'classname'],
			order: [['id', 'DESC'],	[PhotoModel, 'id', 'DESC']],
			include: { model: PhotoModel, 
				attributes: ['url', 'originalname', 'filename'],
			},
		});
		
		if (students.length === 0) {
			return res.status(200).json({
				students: [],
				msg: 'There is no data in the database.',
			});
		}
		
		return res.status(200).json(students);
		} catch (e) {
			return res.status(500).json({
				errors: ['Internal server error.'],
			});
		}
	}
}

export default new StudentControllers();
```
