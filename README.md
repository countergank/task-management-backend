# Backend - Sistema de Gestión de Tareas

## Descripción

API RESTful desarrollada con Node.js, Express.js y TypeScript para el Sistema de Gestión de Tareas. Proporciona endpoints para autenticación, gestión de tareas y categorías con autenticación JWT y base de datos PostgreSQL.

## Stack Tecnológico

- **Node.js** 18+ - Runtime de JavaScript
- **Express.js** - Framework web minimalista
- **TypeScript** - JavaScript tipado
- **TypeORM** - ORM para base de datos
- **PostgreSQL** - Base de datos relacional
- **JWT** - Autenticación basada en tokens
- **bcrypt** - Encriptación de contraseñas
- **Jest** - Testing unitario
- **ESLint/Prettier** - Linting y formateo

## Características

- ✅ Autenticación JWT con refresh tokens
- ✅ CRUD completo para tareas y categorías
- ✅ Validación de datos con Joi
- ✅ Middleware de autenticación y autorización
- ✅ Manejo de errores consistente
- ✅ Logging estructurado
- ✅ Testing unitario e integración
- ✅ Documentación de API
- ✅ CORS configurado
- ✅ Rate limiting
- ✅ Variables de entorno

## Estructura del Proyecto

```
task-management-backend/
├── src/
│   ├── controllers/         # Controladores de rutas
│   │   ├── auth.controller.ts
│   │   ├── task.controller.ts
│   │   └── category.controller.ts
│   ├── models/              # Modelos de datos (TypeORM)
│   │   ├── User.ts
│   │   ├── Task.ts
│   │   └── Category.ts
│   ├── routes/              # Definición de rutas
│   │   ├── auth.routes.ts
│   │   ├── task.routes.ts
│   │   └── category.routes.ts
│   ├── middleware/          # Middleware personalizado
│   │   ├── auth.middleware.ts
│   │   ├── validation.middleware.ts
│   │   └── error.middleware.ts
│   ├── services/            # Lógica de negocio
│   │   ├── auth.service.ts
│   │   ├── task.service.ts
│   │   └── category.service.ts
│   ├── utils/               # Utilidades
│   │   ├── logger.ts
│   │   ├── response.ts
│   │   └── validation.ts
│   ├── types/               # Tipos TypeScript
│   │   ├── auth.types.ts
│   │   ├── task.types.ts
│   │   └── common.types.ts
│   ├── config/              # Configuración
│   │   ├── database.ts
│   │   ├── jwt.ts
│   │   └── app.ts
│   └── app.ts               # Aplicación principal
├── tests/                   # Tests
│   ├── unit/               # Tests unitarios
│   ├── integration/        # Tests de integración
│   └── fixtures/           # Datos de prueba
├── migrations/             # Migraciones de base de datos
├── seeds/                  # Datos de prueba
├── package.json
├── tsconfig.json
├── .env.example
└── README.md
```

## Instalación

### Prerrequisitos

- Node.js 18+
- PostgreSQL 13+
- npm o yarn

### Pasos de Instalación

1. **Clonar el repositorio**
```bash
git clone <repository-url>
cd task-management-backend
```

2. **Instalar dependencias**
```bash
npm install
# o
yarn install
```

3. **Configurar variables de entorno**
```bash
cp .env.example .env
```

Editar el archivo `.env` con tus configuraciones:
```env
# Base de datos
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=password
DB_NAME=task_management

# JWT
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=24h
JWT_REFRESH_SECRET=your-refresh-secret
JWT_REFRESH_EXPIRES_IN=7d

# Servidor
PORT=3001
NODE_ENV=development

# CORS
CORS_ORIGIN=http://localhost:3000
```

4. **Configurar base de datos**
```bash
# Crear base de datos
createdb task_management

# Ejecutar migraciones
npm run migration:run

# Poblar con datos de prueba (opcional)
npm run seed:run
```

5. **Ejecutar en desarrollo**
```bash
npm run dev
```

## Scripts Disponibles

```bash
# Desarrollo
npm run dev              # Ejecutar en modo desarrollo
npm run build            # Compilar TypeScript
npm run start            # Ejecutar en producción

# Base de datos
npm run migration:generate -- --name=MigrationName
npm run migration:run    # Ejecutar migraciones
npm run migration:revert # Revertir última migración
npm run seed:run         # Ejecutar seeds

# Testing
npm run test             # Ejecutar todos los tests
npm run test:unit        # Tests unitarios
npm run test:integration # Tests de integración
npm run test:watch       # Tests en modo watch
npm run test:coverage    # Cobertura de tests

# Linting
npm run lint             # Ejecutar ESLint
npm run lint:fix         # Corregir errores de linting
npm run format           # Formatear código con Prettier
```

## API Endpoints

### Autenticación

#### Registro de Usuario
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "usuario@ejemplo.com",
  "password": "password123",
  "firstName": "Juan",
  "lastName": "Pérez"
}
```

#### Inicio de Sesión
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "usuario@ejemplo.com",
  "password": "password123"
}
```

#### Obtener Usuario Actual
```http
GET /api/auth/me
Authorization: Bearer <token>
```

#### Renovar Token
```http
POST /api/auth/refresh
Content-Type: application/json

{
  "refreshToken": "refresh-token"
}
```

### Tareas

#### Obtener Todas las Tareas
```http
GET /api/tasks
Authorization: Bearer <token>
Query Parameters:
- page: número de página (opcional)
- limit: elementos por página (opcional)
- completed: filtrar por estado (opcional)
- categoryId: filtrar por categoría (opcional)
```

#### Obtener Tarea por ID
```http
GET /api/tasks/:id
Authorization: Bearer <token>
```

#### Crear Nueva Tarea
```http
POST /api/tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Título de la tarea",
  "description": "Descripción opcional",
  "priority": "high",
  "dueDate": "2024-12-31",
  "categoryId": 1
}
```

#### Actualizar Tarea
```http
PUT /api/tasks/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Título actualizado",
  "description": "Descripción actualizada",
  "priority": "medium",
  "completed": true
}
```

#### Cambiar Estado de Tarea
```http
PATCH /api/tasks/:id/toggle
Authorization: Bearer <token>
```

#### Eliminar Tarea
```http
DELETE /api/tasks/:id
Authorization: Bearer <token>
```

### Categorías

#### Obtener Todas las Categorías
```http
GET /api/categories
Authorization: Bearer <token>
```

#### Crear Nueva Categoría
```http
POST /api/categories
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Nombre de la categoría",
  "color": "#FF5733"
}
```

#### Actualizar Categoría
```http
PUT /api/categories/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Nombre actualizado",
  "color": "#33FF57"
}
```

#### Eliminar Categoría
```http
DELETE /api/categories/:id
Authorization: Bearer <token>
```

## Modelos de Datos

### User
```typescript
interface User {
  id: number;
  email: string;
  password: string; // hasheada
  firstName: string;
  lastName: string;
  createdAt: Date;
  updatedAt: Date;
  tasks: Task[];
  categories: Category[];
}
```

### Task
```typescript
interface Task {
  id: number;
  title: string;
  description?: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  dueDate?: Date;
  createdAt: Date;
  updatedAt: Date;
  userId: number;
  user: User;
  categoryId?: number;
  category?: Category;
}
```

### Category
```typescript
interface Category {
  id: number;
  name: string;
  color: string;
  createdAt: Date;
  updatedAt: Date;
  userId: number;
  user: User;
  tasks: Task[];
}
```

## Autenticación

### JWT Tokens
- **Access Token**: Válido por 24 horas
- **Refresh Token**: Válido por 7 días
- Tokens se envían en el header `Authorization: Bearer <token>`

### Middleware de Autenticación
Todas las rutas protegidas requieren el header de autorización:
```http
Authorization: Bearer <access-token>
```

### Renovación de Tokens
Cuando el access token expira, usar el refresh token para obtener uno nuevo:
```http
POST /api/auth/refresh
{
  "refreshToken": "refresh-token"
}
```

## Validación de Datos

### Esquemas de Validación
- **Registro**: email válido, password mínimo 6 caracteres
- **Login**: email y password requeridos
- **Tareas**: título requerido, prioridad enum, fecha opcional
- **Categorías**: nombre requerido, color formato hex

### Respuestas de Error
```json
{
  "error": "Validation Error",
  "message": "Email is required",
  "details": [
    {
      "field": "email",
      "message": "Email is required"
    }
  ]
}
```

## Manejo de Errores

### Códigos de Estado HTTP
- `200` - OK
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `409` - Conflict
- `422` - Unprocessable Entity
- `500` - Internal Server Error

### Formato de Respuesta de Error
```json
{
  "error": "Error Type",
  "message": "Human readable message",
  "statusCode": 400,
  "timestamp": "2024-01-01T00:00:00.000Z",
  "path": "/api/tasks"
}
```

## Testing

### Ejecutar Tests
```bash
# Todos los tests
npm run test

# Tests unitarios
npm run test:unit

# Tests de integración
npm run test:integration

# Con cobertura
npm run test:coverage
```

### Estructura de Tests
```
tests/
├── unit/                   # Tests unitarios
│   ├── services/          # Tests de servicios
│   ├── utils/             # Tests de utilidades
│   └── middleware/        # Tests de middleware
├── integration/           # Tests de integración
│   ├── auth.test.ts       # Tests de autenticación
│   ├── tasks.test.ts      # Tests de tareas
│   └── categories.test.ts # Tests de categorías
└── fixtures/              # Datos de prueba
    ├── users.json
    ├── tasks.json
    └── categories.json
```

## Base de Datos

### Migraciones
```bash
# Generar migración
npm run migration:generate -- --name=CreateUsersTable

# Ejecutar migraciones
npm run migration:run

# Revertir migración
npm run migration:revert
```

### Seeds
```bash
# Ejecutar seeds
npm run seed:run
```

### Configuración de Base de Datos
```typescript
// config/database.ts
export const databaseConfig = {
  type: 'postgres',
  host: process.env.DB_HOST,
  port: parseInt(process.env.DB_PORT),
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  entities: [User, Task, Category],
  synchronize: false, // Solo en desarrollo
  logging: process.env.NODE_ENV === 'development',
  migrations: ['src/migrations/*.ts'],
  seeds: ['src/seeds/*.ts']
};
```

## Logging

### Configuración de Logs
```typescript
// utils/logger.ts
import winston from 'winston';

export const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});
```

### Uso de Logs
```typescript
import { logger } from '../utils/logger';

// Log de información
logger.info('User created successfully', { userId: user.id });

// Log de error
logger.error('Database connection failed', { error: error.message });

// Log de warning
logger.warn('Rate limit exceeded', { ip: req.ip });
```

## Seguridad

### Configuración CORS
```typescript
app.use(cors({
  origin: process.env.CORS_ORIGIN,
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

### Rate Limiting
```typescript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100, // máximo 100 requests por IP
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);
```

### Helmet para Seguridad
```typescript
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: false,
  crossOriginEmbedderPolicy: false
}));
```

## Variables de Entorno

### Archivo .env
```env
# Base de datos
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=password
DB_NAME=task_management

# JWT
JWT_SECRET=your-super-secret-key
JWT_EXPIRES_IN=24h
JWT_REFRESH_SECRET=your-refresh-secret-key
JWT_REFRESH_EXPIRES_IN=7d

# Servidor
PORT=3001
NODE_ENV=development

# CORS
CORS_ORIGIN=http://localhost:3000

# Logging
LOG_LEVEL=info

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

## Deployment

### Producción
```bash
# Instalar dependencias de producción
npm ci --only=production

# Compilar TypeScript
npm run build

# Ejecutar migraciones
npm run migration:run

# Iniciar aplicación
npm start
```

### Docker
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3001

CMD ["npm", "start"]
```

### Variables de Entorno de Producción
```env
NODE_ENV=production
DB_HOST=your-production-db-host
DB_PASSWORD=your-production-password
JWT_SECRET=your-production-secret
CORS_ORIGIN=https://your-frontend-domain.com
```

## Monitoreo y Logs

### Health Check
```http
GET /api/health
```

Respuesta:
```json
{
  "status": "OK",
  "timestamp": "2024-01-01T00:00:00.000Z",
  "uptime": 3600,
  "database": "connected"
}
```

### Métricas
- Tiempo de respuesta de endpoints
- Número de requests por minuto
- Errores por endpoint
- Uso de memoria y CPU

## Troubleshooting

### Problemas Comunes

#### Error de Conexión a Base de Datos
```bash
# Verificar que PostgreSQL esté ejecutándose
sudo systemctl status postgresql

# Verificar credenciales en .env
# Verificar que la base de datos exista
createdb task_management
```

#### Error de JWT
```bash
# Verificar que JWT_SECRET esté configurado
# Verificar formato de token en header
Authorization: Bearer <token>
```

#### Error de CORS
```bash
# Verificar CORS_ORIGIN en .env
# Verificar que el frontend esté en el origen correcto
```

### Logs de Debug
```bash
# Habilitar logs detallados
LOG_LEVEL=debug npm run dev
```

## Contribución

### Flujo de Trabajo
1. Fork del repositorio
2. Crear branch para feature: `git checkout -b feature/nueva-funcionalidad`
3. Hacer commit: `git commit -m 'Add nueva funcionalidad'`
4. Push: `git push origin feature/nueva-funcionalidad`
5. Crear Pull Request

### Estándares de Código
- Seguir convenciones de TypeScript
- Escribir tests para nuevas funcionalidades
- Documentar cambios en README
- Usar conventional commits

## Recursos de Aprendizaje

### Node.js y Express
- [Node.js Official Docs](https://nodejs.org/docs/)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

### Base de Datos
- [TypeORM Documentation](https://typeorm.io/)
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/current/tutorial.html)

### Testing
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [Supertest for API Testing](https://github.com/visionmedia/supertest)

### Seguridad
- [OWASP Node.js Security](https://owasp.org/www-project-nodejs-security/)
- [JWT.io](https://jwt.io/) - Explicación de JSON Web Tokens

---

Para más información, consulta la documentación completa del proyecto en la carpeta `docs/`.
