# 1.2 Fundamentos del funcionamiento de la web

## Introducción

Comprender cómo funcionan los sitios web es esencial desde la perspectiva de la ciberseguridad. En esta sección explicamos cómo funcionan, sus componentes principales y cómo se comunican entre sí.

## Cómo funciona un sitio web

### DNS (Domain Name System)

Para que un sitio funcione, intervienen distintos componentes y procesos automatizados. Primero, el sistema DNS se usa para resolver un dominio (p. ej., google.com) a una dirección IP. Los dominios siguen una jerarquía (subdominios, dominios de segundo nivel, TLDs) y distintos registros DNS (A, AAAA, CNAME, MX, TXT…) aportan diferentes tipos de información.

![DNS](images/dns1.png)

Al realizar una solicitud DNS, primero se consulta la caché local; después, el servidor DNS del ISP, los servidores raíz, los servidores TLD y los servidores autoritativos se activan hasta obtener la IP. La respuesta incluye un TTL (Time To Live) que indica cuánto tiempo puede mantenerse en caché.

![DNS](images/dns2.png)

```
    1. Paso: Tu ordenador
    Primero mira en su caché local. Puede que hayas visitado ese sitio recientemente y recuerde la IP.
    Resultado:
    ✅ Si lo encuentra: Conecta directamente
    ❌ Si no lo encuentra: Pasa al siguiente paso


    2. Paso: Servidor DNS recursivo
    Es el servidor que te proporciona tu ISP (o el que tú elijas).
    También consulta su propia caché local.
    Sitios populares como Google, Facebook o Twitter suelen estar aquí cacheados.
    Resultado:
    ✅ Si lo encuentra: Lo devuelve a tu equipo
    ❌ Si no lo encuentra: Acude a los servidores raíz de Internet


    3. Paso: Servidores raíz (Root DNS)
    Son la “columna vertebral” de Internet. Miran el TLD (.com, .org, .es...) y te envían al servidor TLD adecuado.
    En nuestro ejemplo: para www.tryhackme.com ven .com y te envían al TLD .com.

    4. Paso: Servidor TLD
    Indica dónde está el servidor autoritativo del dominio.
    Por ejemplo, para tryhackme.com los servidores autoritativos son:
    kip.ns.cloudflare.com
    uma.ns.cloudflare.com

    Hay al menos dos para redundancia.

    5. Paso: Servidor DNS autoritativo
    Servidor que almacena todos los registros DNS de ese dominio.
    ¿Qué hace?
    Busca la IP necesaria (registro A)
    La devuelve al DNS recursivo
    El recursivo la cachea para futuras peticiones
    Finalmente la envía a tu ordenador
```

### HTTP en detalle

Una vez obtenida la IP, se realiza una solicitud HTTP o HTTPS al servidor.

Es el conjunto de reglas para comunicarse con servidores web y transmitir datos.

Puede transportar muchos tipos de datos:

- HTML (contenido del sitio)
- Imágenes (JPG, PNG, GIF...)
- Vídeos (MP4, AVI...)
- Otros archivos (PDF, documentos...)

HTTPS es la versión segura que cifra la comunicación.

Las solicitudes HTTP incluyen cabeceras (Host, User-Agent, Cookie...), y el servidor devuelve una respuesta con un código de estado (200, 404, 500...) y un cuerpo (HTML, JSON, imágenes...).

La estructura de una URL incluye protocolo, usuario, host, puerto, ruta, parámetros y fragmento. Según la solicitud, el servidor devuelve un recurso concreto.

Tomemos este ejemplo: <http://user:password@tryhackme.com:80/view-room?id=1#task3>

![URL](images/url1.png)

- Scheme (Esquema): indica el protocolo a usar (http, https, ftp...).
- User (Usuario): user:password para servicios que requieren credenciales (hoy casi no se usa por no ser seguro).
- Host (Host): dominio o IP del servidor (p. ej., google.com o 192.168.1.1).
- Port (Puerto): número de puerto (HTTP 80 por defecto, HTTPS 443).
- Path (Ruta): ubicación del recurso (/blog o /images/photo.jpg).
- Query String (Cadena de consulta): información adicional (?id=1&category=tech).
- Fragment (Fragmento): parte específica de la página (#task3).

#### Métodos HTTP

Los métodos indican la intención del cliente al hacer una solicitud. Los más comunes son:

- GET: Obtener información del servidor.
- POST: Enviar datos al servidor y, posiblemente, crear registros.
- PUT: Enviar datos para actualizar información existente.
- DELETE: Eliminar información o registros del servidor.

#### Códigos de estado HTTP

Indican el estado de la respuesta del servidor. Se agrupan en:

- 100-199: Informativos
- 200-299: Éxito
- 300-399: Redirección
- 400-499: Errores del cliente
- 500-599: Errores del servidor

#### Cabeceras

Pequeños datos adicionales que se envían con la solicitud. Algunas comunes:

- Solicitud: Host, User-Agent, Content-Length, Accept-Encoding, Cookie
- Respuesta: Set-Cookie, Cache-Control, Content-Type, Content-Encoding

#### Cookies

Las cookies son pequeños datos que se almacenan en tu equipo. Se guardan cuando el servidor envía la cabecera Set-Cookie. En solicitudes posteriores, se devuelven al servidor. Dado que HTTP es sin estado, las cookies ayudan a recordar quién eres, guardar preferencias o saber si ya visitaste el sitio.

### ¿Cómo funcionan los sitios web?

Cuando visitas una web, tu navegador realiza una solicitud al servidor pidiendo la información de la página. El servidor envía los datos que el navegador usará para mostrar el contenido. Un servidor web es un ordenador dedicado que procesa estas solicitudes.

![¿Cómo funcionan los sitios web?](images/nola_funtzionatu.png)

Frontend vs Backend: el frontend es lo que ves en el navegador (HTML, CSS, JS) y el backend es lo que se ejecuta en el servidor (PHP, Node.js...). El contenido puede ser estático o dinámico y un servidor puede gestionar varios sitios mediante Virtual Host.

HTML, CSS y JavaScript son las tecnologías base del front. Los elementos tienen atributos (id, class, src...), y si usan entrada de usuario pueden ser peligrosos si no se filtra correctamente.

La exposición de datos sensibles ocurre a menudo en el código del front (comentarios, ficheros JS...) donde se dejan contraseñas, tokens o enlaces privados. También pueden surgir vulnerabilidades como la inyección de HTML si no se filtra y se muestra directamente en la página.

Las herramientas de desarrollo del navegador (Developer Tools) son útiles para inspeccionar solicitudes, cookies, código y obtener información relevante de seguridad (Inspector, Network, Debugger...).

### Otros componentes importantes

CDNs: ayudan a reducir el tráfico sirviendo archivos estáticos desde servidores distribuidos.

Balanceadores de carga: reparten el tráfico entre varios servidores cuando la carga aumenta o se necesita alta disponibilidad.

WAFs (Web Application Firewall): detectan y bloquean solicitudes maliciosas.

Bases de datos: almacenan y proporcionan datos a las aplicaciones.

![WAF](images/firewall1.png)

## Próximos pasos

- [Visión general de vulnerabilidades comunes](owasp_top10.md)
- [Volver al inicio](../index.md)
