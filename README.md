# BiblioIcesi API

Sistema de gestión de biblioteca desarrollado con Node.js, TypeScript y MongoDB.

## 🚀 Características

- **Gestión de Usuarios**: Autenticación JWT, roles y permisos (RBAC)
- **Catálogo de Libros**: CRUD completo con validación ISBN
- **Gestión de Ejemplares**: Control de disponibilidad y estado
- **Sistema de Préstamos**: Creación, devolución y cálculo de mora
- **Reservas**: Sistema de reservas con expiración automática
- **Integración Google Books**: Enriquecimiento de metadatos
- **Reportes**: Estadísticas y exportación de datos
- **Tests**: Cobertura de código del 64%+ con tests unitarios e integración

## 🛠️ Stack Tecnológico

- **Backend**: Node.js + Express + TypeScript
- **Base de Datos**: MongoDB (desarrollo local) / MongoDB Atlas (producción)
- **Autenticación**: JWT + bcrypt
- **Testing**: Jest + Supertest + MongoDB Memory Server
- **Despliegue**: Vercel (API) + MongoDB Atlas
- **Integración**: Google Books API

## 📋 Prerrequisitos

- Node.js 18+
- MongoDB (local o Atlas)
- npm o yarn

## 🔧 Instalación

1. **Clonar el repositorio**
```bash
git clone <repository-url>
cd biblioicesi-api
```

2. **Instalar dependencias**
```bash
npm install
```

3. **Configurar variables de entorno**
```bash
cp env.example .env
```

Editar `.env` con tus configuraciones:
```env
NODE_ENV=development
PORT=3000
SECRET=your_jwt_secret_key_here
MONGODB_URI=mongodb+srv://<user>:<pass>@cluster0.xxxxx.mongodb.net/biblioicesi?retryWrites=true&w=majority
MONGODB_DB=biblioicesi
MONGODB_TEST_URI=mongodb://localhost:27017/biblioicesi_test
GOOGLE_BOOKS_API_KEY=your_google_books_api_key_here
```

4. **Configurar índices de base de datos**
```bash
npm run ensure-indexes
```

5. **Sembrar datos iniciales (RBAC + admin)**
```bash
npm run seed
```

6. **Iniciar servidor de desarrollo**
```bash
npm run dev
```

## 🧪 Testing

### Tests Unitarios
```bash
# Ejecutar todos los tests
npm test

# Tests con cobertura
npm run test:coverage

# Tests en modo watch
npm run test:watch
```

### Tests de Integración
```bash
# Tests de integración con base de datos
npm run test:integration
```
<img width="890" height="421" alt="image" src="https://github.com/user-attachments/assets/5894fb56-e56e-44a9-9c47-a509cec0d091" />

POSTMAN

<img width="1098" height="567" alt="image" src="https://github.com/user-attachments/assets/a0cb9528-dc25-4816-9c16-5142715db208" />

## 📚 API Endpoints

### Autenticación
- `POST /api/auth/login` - Iniciar sesión
- `POST /api/auth/register` - Registro de usuario

### Usuarios
- `GET /api/users` - Listar usuarios (Admin)
- `GET /api/users/profile` - Perfil del usuario
- `PUT /api/users/profile` - Actualizar perfil
- `DELETE /api/users/:id` - Eliminar usuario (Admin)

### Libros
- `GET /api/books` - Listar libros
- `GET /api/books/:id` - Obtener libro
- `POST /api/books` - Crear libro (Admin)
- `PUT /api/books/:id` - Actualizar libro (Admin)
- `DELETE /api/books/:id` - Eliminar libro (Admin)

### Ejemplares
- `GET /api/copies` - Listar ejemplares
- `GET /api/copies/available` - Ejemplares disponibles
- `POST /api/copies` - Crear ejemplar (Admin)
- `PUT /api/copies/:id/status` - Actualizar estado

### Préstamos
- `GET /api/loans` - Listar préstamos
- `POST /api/loans` - Crear préstamo
- `PUT /api/loans/:id/return` - Devolver préstamo
- `GET /api/loans/my` - Mis préstamos

### Google Books
- `GET /api/google-books/search?q=query` - Buscar libros
- `GET /api/google-books/isbn/:isbn` - Obtener por ISBN
- `POST /api/google-books/enrich` - Enriquecer datos

## 🌐 Despliegue en Vercel

### 1. Configuración de Base de Datos

**MongoDB Atlas (Recomendado)**
1. Crear cuenta en [MongoDB Atlas](https://www.mongodb.com/atlas)
2. Crear cluster
3. Configurar red IP (0.0.0.0/0 para Vercel)
4. Obtener connection string

### 2. Variables de Entorno en Vercel

```bash
# Instalar Vercel CLI
npm i -g vercel

# Configurar variables
vercel env add MONGODB_URI
vercel env add MONGODB_DB
vercel env add SECRET
vercel env add GOOGLE_BOOKS_API_KEY
vercel env add NODE_ENV production
```

### 3. Despliegue

```bash
# Desplegar
vercel --prod

# O con Git
git push origin main
```
Link desplegado: https://biblio-icesi-iwl3vkuv0-santiagos-projects-a3f1c6d3.vercel.app/
### 4. Sembrar datos en producción

```bash
# Conectar a Vercel y ejecutar seed
vercel env pull .env.production
npm run seed
```

## 🔒 Seguridad

- **Autenticación JWT**: Tokens seguros con expiración
- **Hashing de contraseñas**: bcrypt con salt rounds
- **Validación de entrada**: Zod schemas
- **Rate Limiting**: Protección contra ataques
- **CORS**: Configuración de orígenes permitidos
- **Variables de entorno**: Configuración segura

## 📊 Monitoreo y Logs

### Health Check
```bash
GET /health
```

### Logs en Vercel
```bash
vercel logs
```

## 🤝 Contribución

1. Fork el proyecto
2. Crear rama feature (`git checkout -b feature/AmazingFeature`)
3. Commit cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abrir Pull Request

## 📝 Estructura del Proyecto

```
src/
├── config/          # Configuración de base de datos
├── controllers/     # Controladores de la API
├── interfaces/      # Interfaces TypeScript
├── middlewares/     # Middlewares de autenticación y validación
├── models/          # Modelos de MongoDB
├── routes/          # Rutas de la API
├── schemas/         # Esquemas de validación
├── services/        # Lógica de negocio
└── index.ts         # Punto de entrada

__tests__/
├── integration/     # Tests de integración
├── postman/         # Colección de Postman
└── *.test.ts        # Tests unitarios

scripts/
├── migrate.js       # Migraciones de base de datos
└── mongo-init.js    # Inicialización de MongoDB
```

## 🐛 Troubleshooting

### Error de conexión a MongoDB
```bash
# Verificar que MongoDB esté corriendo
npm run docker:logs

# Verificar variables de entorno
echo $MONGODB_URI
```

### Tests fallando
```bash
# Limpiar cache de Jest
npm test -- --clearCache

# Ejecutar tests específicos
npm test -- --testNamePattern="UserService"
```

### Error en Vercel
```bash
# Verificar logs
vercel logs

# Verificar variables de entorno
vercel env ls
```

## 📄 Licencia

Este proyecto está bajo la Licencia ISC.

## 👥 Autores

- Alejandro Castro
- Santiago Cardenas

## 🙏 Agradecimientos

- Google Books API por el enriquecimiento de metadatos
- MongoDB Atlas por el hosting de base de datos
- Vercel por el hosting de la API
