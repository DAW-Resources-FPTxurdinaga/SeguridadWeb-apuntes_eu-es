# 3.1 Gestión y protección de datos sensibles

## Cifrado de datos

### ¿Por qué es importante cifrar?
- Si roban los datos, no podrán leerlos
- Asegura la protección durante la transmisión
- Cumplimiento normativo (GDPR, HIPAA, etc.)

### Tipos de cifrado

#### 1. Cifrado simétrico (AES)
Se usa la misma clave para cifrar y descifrar.

Ejemplo en Python (PyCryptodome):
```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad
import base64

def cifrar(mensaje, clave=None):
    if clave is None:
        clave = get_random_bytes(32)  # 256 bits
    iv = get_random_bytes(16)
    cipher = AES.new(clave, AES.MODE_CBC, iv)
    bloqueado = pad(mensaje.encode('utf-8'), AES.block_size)
    texto_cifrado = cipher.encrypt(bloqueado)
    return base64.b64encode(iv + texto_cifrado).decode('utf-8')

# ... función descifrar análoga ...
```

#### 2. Cifrado asimétrico (RSA)
Usa un par de claves pública/privada.

Ejemplo en Python (cryptography):
```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import serialization, hashes

def generar_claves():
    privada = rsa.generate_private_key(public_exponent=65537, key_size=4096)
    publica = privada.public_key()
    return privada, publica
```

## Transmisión de datos (TLS/HTTPS)

### Configurar HTTPS en Node.js (Express)
```javascript
const https = require('https');
const fs = require('fs');
const express = require('express');
const helmet = require('helmet');

const app = express();
app.use(helmet());

const options = {
  key: fs.readFileSync('path/to/private.key'),
  cert: fs.readFileSync('path/to/certificate.crt'),
  minVersion: 'TLSv1.2'
};

https.createServer(options, app).listen(443, () => {
  console.log('Servidor HTTPS en 443...');
});
```

## Consejos para almacenamiento

- No almacenes datos sensibles si no es necesario
- Minimiza datos recopilados y almacenados
- Enmascara datos al mostrarlos

```javascript
function enmascararCuenta(n) {
  if (!n) return '';
  return '*'.repeat(Math.max(0, n.length - 4)) + n.slice(-4);
}
```

## Gestión segura de tokens

- Prefiere cookies HTTP-only seguras para almacenar tokens
- Define expiración adecuada y rotación de tokens

```javascript
res.cookie('token', token, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 24 * 60 * 60 * 1000
});
```

## Ejercicio práctico

1. Implementa cifrado simétrico para datos sensibles
2. Configura HTTPS en tu proyecto
3. Diseña un sistema de tokens con cookies HTTP-only
4. Verifica medidas de seguridad (helmet, CSP, etc.)

## Próximos pasos

- [APIs seguras y autenticación](api_segurtasuna.md)
- [Volver a la sección anterior](../eraso_defentsak/sarbide_kontrola.md)
