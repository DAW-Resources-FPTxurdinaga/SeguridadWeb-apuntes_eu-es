# 1.3 Visión general de vulnerabilidades comunes (OWASP Top 10)

## ¿Qué es el OWASP Top 10?

OWASP (Open Web Application Security Project) publica y actualiza una lista con los 10 riesgos más críticos en aplicaciones web. La versión vigente es la de 2021, con intención de actualizarse próximamente.

<a href="https://owasp.org/www-project-top-ten/" target="_blank">OWASP Top 10</a>

![OWASP Top 10](images/owasp_top10.png)
> En la imagen anterior pueden verse las diferencias entre 2017 y 2021.

## 1. Broken Access Control (Control de acceso roto)

Se produce cuando se falla al limitar que los usuarios sólo puedan realizar acciones permitidas.
<a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/" target="_blank">OWASP Top 10 - Broken Access Control</a>

### Ejemplos
- Acceso a archivos privados sin permiso
- Editar datos de otros usuarios
- Acceso a funciones administrativas sin autorización

### Prevención
- Aplicar control de acceso con prioridad
- Empezar con postura de “no permitido” por defecto
- Verificar el acceso tanto en cliente como en servidor (especialmente en servidor)

### Retos

- [Broken Access Control](../ariketak/broken_access_control.md)

## 2. Cryptographic Failures (Fallos criptográficos)

Debilidades o fallos en el cifrado de datos.
<a href="https://owasp.org/Top10/A02_2021-Cryptographic_Failures/" target="_blank">OWASP Top 10 - Cryptographic Failures</a>

### Ejemplos
- Transmitir datos sin cifrado
- Usar cifrado débil (MD5, SHA1, DES...)
- Mala gestión de claves privadas

### Prevención
- Usar estándares de cifrado vigentes (AES, RSA...)
- Usar siempre HTTPS
- Guardar claves de forma segura

### Retos

- [Cryptographic Failures](../ariketak/cryptographic_failures.md)

## 3. Injection (Inyección)

Uno de los riesgos más graves. Un atacante introduce datos maliciosos en comandos o consultas interpretados por la aplicación. Puede tomar control, acceder a datos personales o manipular la base de datos.
<a href="https://owasp.org/Top10/A03_2021-Injection/" target="_blank">OWASP Top 10 - Injection</a>

### Ejemplos
- SQL Injection (inyección SQL):

```sql
-- Ejemplo: Entrada del usuario: ' OR '1'='1' --
SELECT * FROM users WHERE username = '' OR '1'='1' -- AND password = '...'
```

- OS Command Injection (inyección de comandos del SO):

```bash
# Ejemplo
ping 127.0.0.1 ; ls -l
```

- LDAP Injection:

```bash
# Ejemplo
*)(uid=*))(|(uid=*
```

### Prevención
- Usar consultas parametrizadas (prepared statements)
- Preferir ORM seguros evitando concatenaciones SQL
- Validar y sanear todas las entradas
- Aplicar principio de mínimo privilegio

### Retos

- [Injection](../ariketak/injection.md)

## 4. Insecure Design (Diseño inseguro)

Problemas de seguridad que nacen en el diseño, no sólo en la implementación.
<a href="https://owasp.org/Top10/A04_2021-Insecure_Design/" target="_blank">OWASP Top 10 - Insecure Design</a>

### Ejemplos
- Diseños sin criterios de seguridad
- Ausencia de modelos de amenazas
- Falta de mecanismos contra abusos

### Prevención
- Incluir seguridad desde el inicio del diseño
- Elaborar modelos de amenazas
- Identificar y prevenir casos de abuso

### Retos

- [Insecure Design](../ariketak/insecure_design.md)

## 5. Security Misconfiguration (Configuración insegura)

Ausencia de configuración adecuada o configuración errónea.
<a href="https://owasp.org/Top10/A05_2021-Security_Misconfiguration/" target="_blank">OWASP Top 10 - Security Misconfiguration</a>

### Ejemplos
- Dejar configuraciones por defecto
- Mostrar mensajes de error detallados
- No activar controles de seguridad adicionales

### Prevención
- Establecer configuración segura
- Automatizar la configuración
- Revisar y actualizar con frecuencia

### Retos

- [Security Misconfiguration](../ariketak/security_misconfiguration.md)

## 6. Vulnerable and Outdated Components (Componentes vulnerables/obsoletos)

Vulnerabilidades en librerías y componentes de terceros.
<a href="https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/" target="_blank">OWASP Top 10 - Vulnerable and Outdated Components</a>

### Ejemplos
- Desconocer vulnerabilidades conocidas
- Falta de actualizaciones
- Uso de componentes inseguros

### Prevención
- Actualizar componentes con frecuencia
- Evitar componentes obsoletos
- Escanear componentes en busca de vulnerabilidades

### Retos

- [Vulnerable and Outdated Components](../ariketak/vulnerable_obsolete_components.md)

## 7. Identification and Authentication Failures (Fallas de identificación y autenticación)

Problemas para identificar y autenticar usuarios.
<a href="https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/" target="_blank">OWASP Top 10 - Identification and Authentication Failures</a>

### Ejemplos
- Aceptar contraseñas débiles
- Mala gestión de sesión
- Funcionalidades de autenticación débiles

### Prevención
- Implementar MFA
- Políticas robustas de contraseñas
- Verificar identificadores de sesión

### Retos

- [Identification and Authentication Failures](../ariketak/identification_and_authentication_failures.md)

## 8. Software and Data Integrity Failures (Fallas de integridad de software y datos)

Riesgos cuando no se verifica la integridad y procedencia de código y datos.
<a href="https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/" target="_blank">OWASP Top 10 - Software and Data Integrity Failures</a>

### Ejemplos
- Modificaciones no autorizadas de código o datos
- Entradas de usuario no verificadas
- Código sin firma digital

### Prevención
- Usar firmas digitales
- Verificar origen de código y datos
- Implementar comprobaciones de integridad

### Retos

- [Software and Data Integrity Failures](../ariketak/software_and_data_integrity_failures.md)

## 9. Security Logging and Monitoring Failures (Fallas de registro y monitoreo)

Falta de capacidad para detectar y responder a incidentes.
<a href="https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/" target="_blank">OWASP Top 10 - Security Logging and Monitoring Failures</a>

### Ejemplos
- Sin configuración de registros
- Falta de alertas
- Ausencia de procedimientos de respuesta

### Prevención
- Establecer un sistema de logs adecuado
- Configurar alertas automáticas
- Planificar procedimientos de respuesta

### Retos

- [Security Logging and Monitoring Failures](../ariketak/security_logging_monitoring_failures.md)

## 10. Server-Side Request Forgery (SSRF)

El atacante controla las solicitudes que el servidor realiza dentro de la red interna.
<a href="https://owasp.org/Top10/A10_2021-Server_Side_Request_Forgery/" target="_blank">OWASP Top 10 - Server-Side Request Forgery</a>

### Ejemplos
- Solicitudes a servicios internos
- Acceso a información de configuración
- Acceso a recursos de la red interna

### Prevención
- Validar y filtrar todas las URLs de entrada
- Usar listas blancas de destinos permitidos
- Limitar conexiones y tiempos

### Retos

- [Server-Side Request Forgery](../ariketak/server_side_request_forgery.md)

## Próximos pasos

- [Defensas frente a inyecciones](../../eraso_defentsak/injekzioak.md)
