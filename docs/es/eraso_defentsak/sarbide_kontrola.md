# 2.3 Control de acceso y configuración segura

## Tipos de control de acceso

### 1. RBAC (Control de acceso basado en roles)
Los usuarios se clasifican por roles y cada rol tiene un conjunto de permisos.

Ejemplo (Node.js + Express):
```javascript
// Definición de roles
const ROLES = {
  ADMIN: 'admin',
  EDITOR: 'editor',
  USER: 'user',
  ENVIAR: 'enviar'
};

// Permisos por rol
const PERMISSIONS = {
  [ROLES.ADMIN]: ['leer', 'escribir', 'eliminar', 'editar', 'enviar'],
  [ROLES.EDITOR]: ['leer', 'escribir', 'enviar'],
  [ROLES.USER]: ['leer', 'enviar'],
  [ROLES.ENVIAR]: ['enviar']
};

// Middleware para verificar permisos
function checkPermission(accion) {
  return (req, res, next) => {
    const rolUsuario = req.user.rol; // viene del middleware de autenticación

    if (!rolUsuario || !PERMISSIONS[rolUsuario] || !PERMISSIONS[rolUsuario].includes(accion)) {
      return res.status(403).json({ error: 'No tienes permiso para esta acción' });
    }
    next();
  };
}

// Uso de ejemplo
app.delete('/api/usuarios/:id',
  authenticateUser,
  checkPermission('eliminar'),
  (req, res) => {
    // Código para eliminar usuario
  }
);
```

### 2. ABAC (Control de acceso basado en atributos)
Decide el acceso según atributos del usuario y del recurso.

Ejemplo (Python + Policy):
```python
from functools import wraps

def check_access(usuario, recurso, accion):
    if accion == "leer" and recurso.dueno == usuario.id:
        return True
    if accion == "editar" and usuario.rol == "admin":
        return True
    if accion == "enviar" and usuario.estado == "activo":
        return True
    return False

def access_required(accion):
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            usuario = get_jwt_identity()
            recurso = get_recurso(kwargs['recurso_id'])

            if not check_access(usuario, recurso, accion):
                return {"error": "No autorizado"}, 403

            return f(*args, **kwargs)
        return decorated_function
    return decorator
```

## Ataques IDOR (Insecure Direct Object Reference)

### ¿Qué es IDOR?
Un fallo en el que el usuario accede directamente a recursos que no le pertenecen.

### ¿Cómo prevenirlo?

1. Validar el acceso por recurso
```javascript
app.get('/api/usuarios/:id', async (req, res) => {
  try {
    const usuario = await Usuario.findById(req.params.id);
    if (req.user.id !== usuario.id && req.user.rol !== 'admin') {
      return res.status(403).json({ error: 'No autorizado' });
    }
    res.json(usuario);
  } catch (error) {
    res.status(404).json({ error: 'Usuario no encontrado' });
  }
});
```

2. Usar UUID o tokens seguros en lugar de IDs secuenciales
```python
import uuid
from django.db import models

class Documento(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    nombre = models.CharField(max_length=255)
    contenido = models.TextField()
    dueno = models.ForeignKey(User, on_delete=models.CASCADE)
```

## Configuración segura

### Configuración por entorno

config/development.js
```javascript
module.exports = {
  database: { host: 'localhost', port: 5432, name: 'miapp_dev', user: 'postgres' },
  server: { port: 3000, debug: true, cors: { origin: ['http://localhost:8080'] } },
  auth: { jwtSecret: 'clave_dev', passwordSaltRounds: 10 }
};
```

config/production.js
```javascript
module.exports = {
  database: {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    name: process.env.DB_NAME,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD
  },
  server: { port: process.env.PORT || 3000, debug: false, cors: { origin: ['https://tuapp.es'] } },
  auth: { jwtSecret: process.env.JWT_SECRET, passwordSaltRounds: 12 }
};
```

### Principios de configuración segura

1. No commitear secretos
2. Endurecer cabeceras HTTP
```http
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000; includeSubDomains
```
3. Logging seguro (sin datos sensibles)

## Ejercicio práctico

1. Implementar RBAC
2. Agregar defensas IDOR
3. Configurar entornos (dev, prod)
4. Verificar cabeceras seguras

## Próximos pasos

- [Gestión de datos sensibles](../../datu_babesa/datu_sentikorrak.md)
- [Volver a la sección anterior](autentifikazioa.md)
