# 2.2 Seguridad de autenticación y sesiones

## Gestión segura de contraseñas

### ¿Por qué no es suficiente almacenar contraseñas en texto claro?
- Si roban los datos, las contraseñas quedarían expuestas
- Los usuarios reutilizan contraseñas en varios servicios
- Ni siquiera los usuarios legítimos deberían poder verlas

### Hashear contraseñas

#### Usando bcrypt (Python)

```python
import bcrypt

def crear_contrasena(contrasena: str) -> bytes:
    # Generar sal aleatoria
    sal = bcrypt.gensalt()
    # Hashear contraseña
    hash_bytes = bcrypt.hashpw(contrasena.encode('utf-8'), sal)
    return hash_bytes

def verificar_contrasena(contrasena: str, hash_almacenado: bytes) -> bool:
    return bcrypt.checkpw(contrasena.encode('utf-8'), hash_almacenado)

# Ejemplo
contrasena_usuario = "mi_secreto"
hash_bytes = crear_contrasena(contrasena_usuario)
print(f"Hash almacenado: {hash_bytes.decode()}")

print("Contraseña incorrecta:", verificar_contrasena("otra", hash_bytes))  # False
print("Contraseña correcta:", verificar_contrasena(contrasena_usuario, hash_bytes))  # True
```

### ¿Por qué es importante la sal (Salt)?
- Mismas contraseñas producen hashes distintos
- Inutiliza tablas arcoíris
- Evita identificar usuarios con mismo hash

## Gestión segura de sesiones

### Configurar cookies seguras

Ejemplo en Express.js:
```javascript
const session = require('express-session');
const crypto = require('crypto');

const SESSION_SECRET = crypto.randomBytes(32).toString('hex');

app.use(session({
  secret: SESSION_SECRET,
  name: 'sessionId',
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,
    httpOnly: true,
    sameSite: 'lax',
    maxAge: 1000 * 60 * 60 * 24,
    domain: '.tu-dominio.es'
  }
}));
```

### JWT (JSON Web Tokens)

Crear y verificar un token (Node.js):
```javascript
const jwt = require('jsonwebtoken');
const crypto = require('crypto');

const JWT_SECRET = process.env.JWT_SECRET || crypto.randomBytes(64).toString('hex');

function crearToken(usuario) {
  return jwt.sign(
    { sub: usuario.id, email: usuario.email },
    JWT_SECRET,
    { expiresIn: '1h', issuer: 'TuApp', audience: 'tuapp.es' }
  );
}

function verificarToken(token) {
  try {
    const decoded = jwt.verify(token, JWT_SECRET, { issuer: 'TuApp', audience: 'tuapp.es' });
    return { valido: true, contenido: decoded };
  } catch (error) {
    return { valido: false, error: error.message };
  }
}
```

## Autenticación multifactor (MFA)

### ¿Por qué usar MFA?
- Aunque la contraseña se comprometa, evita el acceso
- Combina “algo que sabes”, “algo que tienes”, “algo que eres”

### Tipos de MFA
1. Aplicaciones autenticadoras (Google Authenticator, Authy, Microsoft Authenticator)
2. SMS (menos seguro por ataques SIM swap)
3. Llaves hardware (YubiKey, Titan)
4. Aplicaciones corporativas de aprobación

### Implementación OTP

```python
import pyotp
import qrcode

def configurar_mfa(usuario):
    clave = pyotp.random_base32()
    totp_uri = pyotp.totp.TOTP(clave).provisioning_uri(name=usuario, issuer_name='Tu App')
    qr = qrcode.make(totp_uri)
    qr.save(f'mfa_{usuario}.png')
    return clave

def verificar_codigo(clave, codigo_usuario):
    totp = pyotp.TOTP(clave)
    return totp.verify(codigo_usuario)
```

## Consejos para la seguridad de sesión

1. Usar identificadores largos y aleatorios
2. Configurar expiración adecuada
3. Limitar a una sesión por usuario si aplica
4. Ofrecer botón de cierre de sesión
5. Monitorear ubicación y navegador habituales
6. Mostrar sesiones activas al usuario

## Próximos pasos

- [Control de acceso y configuración segura](sarbide_kontrola.md)
- [Volver a la sección anterior](injekzioak.md)
