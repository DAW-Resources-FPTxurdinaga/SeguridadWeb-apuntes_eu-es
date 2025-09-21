# 2.1 Defensas frente a inyecciones

## Inyección SQL

### ¿Qué es la inyección SQL?
La inyección SQL es un tipo de ataque que modifica una consulta a la base de datos cuando la entrada del usuario se inserta en la consulta sin validación adecuada.

### ¿Cómo funciona?
```sql
-- Entrada del usuario: ' OR '1'='1
-- Consulta original:
SELECT * FROM users WHERE username = '' AND password = 'hasia'

-- Tras la inyección:
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = 'hasia'
-- Como '1'='1' siempre es verdadero, la consulta devolverá todos los usuarios
```

### ¿Cómo prevenirla?

#### 1. Usar consultas parametrizadas (Prepared Statements)

**Ejemplo en Python (SQLite):**
```python
import sqlite3

def login_seguro(usuario, contrasena):
    conn = sqlite3.connect('datu_basea.db')
    cursor = conn.cursor()
    
    # Consulta parametrizada
    consulta = "SELECT * FROM users WHERE username = ? AND password = ?"
    cursor.execute(consulta, (usuario, contrasena))
    
    usuario_row = cursor.fetchone()
    conn.close()
    return usuario_row
```

**Ejemplo en PHP (PDO):**
```php
<?php
$usuario = $_POST['erabiltzailea'];
$contrasena = $_POST['pasahitza'];

$pdo = new PDO('mysql:host=localhost;dbname=nire_db', 'erabiltzailea', 'pasahitza');
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :usuario AND password = :contrasena');
$stmt->execute(['usuario' => $usuario, 'contrasena' => $contrasena]);
$fila = $stmt->fetch();
?>
```

## XSS (Cross-Site Scripting)

### Tipos de XSS

1. Reflected XSS (Ispatuta)
2. Stored XSS (Gordeta)
3. DOM XSS

### ¿Cómo prevenir XSS?

#### 1. Codificar la salida

```javascript
const escapeHtml = (text) => {
  const map = {
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&#x27;',
    "/": '&#x2F;'
  };
  return text.replace(/[&<>"'/]/g, m => map[m]);
};

// Uso
document.getElementById('izena').textContent = escapeHtml(sarreraArriskutsua);
```

#### 2. Usar APIs seguras

```javascript
// Mal
document.getElementById('izena').innerHTML = entradaUsuario;

// Mejor
document.getElementById('izena').textContent = entradaUsuario;
```

#### 3. Content Security Policy (CSP)

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://apis.google.com
```

## Otras inyecciones

### Inyección de comandos

Ejemplo inseguro (Python):
```python
import os

def ping_direccion(direccion):
    os.system(f"ping -c 4 {direccion}")
```

Ejemplo seguro:
```python
import subprocess

def ping_direccion_segura(direccion):
    subprocess.run(['ping', '-c', '4', direccion], check=True)
```

### Inyección LDAP

Ejemplo inseguro:
```python
import ldap

def autenticar(usuario, contrasena):
    consulta = f"(uid={usuario})(userPassword={contrasena})"
```

Ejemplo seguro:
```python
import ldap
from ldap.filter import escape_filter_chars

def autenticar_seguro(usuario, contrasena):
    usuario_ok = escape_filter_chars(usuario)
    pass_ok = escape_filter_chars(contrasena)
    consulta = f"(uid={usuario_ok})(userPassword={pass_ok})"
```

## Ejercicio práctico

1. Crea una página simple con usuario y contraseña
2. Implementa protección frente a inyección SQL
3. Añade defensa XSS en entrada y salida
4. Prueba con entradas maliciosas

## Próximos pasos

- [Seguridad de autenticación y sesiones](autentifikazioa.md)
- [Volver a OWASP Top 10](../oinarriak_mehatxuak/owasp_top10.md)
