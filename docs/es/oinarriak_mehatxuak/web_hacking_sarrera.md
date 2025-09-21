# Introducción al Web Hacking

## Walking An Application

Veremos cómo analizar manualmente una aplicación web para encontrar problemas de seguridad usando sólo las herramientas integradas del navegador. A menudo, las herramientas automatizadas y scripts pasan por alto vulnerabilidades potenciales e información útil.

Resumen de herramientas del navegador que usarás a lo largo del módulo:

- Ver código fuente (View Source)
- Inspector (Inspector)
- Depurador (Debugger)
- Red (Network)

## Exploring The Website

Como pentester, tu objetivo al analizar un sitio o aplicación web es localizar características potencialmente vulnerables y tratar de explotarlas para evaluar si lo son. Estas suelen requerir interacción del usuario. Puedes empezar explorando con el navegador y tomando notas de páginas/áreas/funcionalidades con un breve resumen de cada una.

## Ver el código fuente de la página

Cuando visitamos una web, el navegador realiza una solicitud al servidor, que devuelve **código**: el page source o **código fuente de la página**. Ese código indica al navegador qué contenido mostrar, cómo presentarlo y cómo debe comportarse.

En el código fuente suele haber **HTML, CSS y JavaScript**:

- HTML para estructura y contenido
- CSS para apariencia (colores, tamaños…)
- JavaScript para funcionalidades interactivas

Mirar el código fuente puede aportar **información extra**, especialmente desde el punto de vista de la seguridad.

### ¿Cómo veo el código fuente?

- Clic derecho en la página y elegir “Ver código fuente” o similar
- Prefijar la URL del navegador con: `view-source:https://www.ejemplo.com`
- En el menú del navegador, en “Developer tools” o “Más herramientas”

### ¿Qué elementos buscar?

- Comentarios (`<!-- ... -->`): pueden dar pistas
- Enlaces (`<a href="...">`): navegación, enlaces ocultos
- Archivos externos (CSS, JS, imágenes...)
- Pistas del framework y su versión

## Herramientas de desarrollo (Developer Tools)

### 1. Inspector

Muestra el “fotograma real” de lo que se renderiza. Permite cambiar elementos (texto, colores, tamaños) para facilitar depuración y descubrimiento de fallos.

### 2. Debugger

Permite seguir el flujo del JavaScript con breakpoints para inspeccionar valores y entender por qué ocurren errores. Dominarlo permite controlar la ejecución del código del cliente.

### 3. Network

Muestra cada solicitud externa que realiza la página. Recarga la página con la pestaña abierta para ver todo lo que se solicita.

## Descubrimiento de contenidos

### ¿Qué es el descubrimiento de contenidos?

Contenido no presentado de forma evidente: portales internos, versiones antiguas, copias de seguridad, archivos de configuración, paneles de administración… Puede encontrarse de forma manual, automatizada u OSINT.

### Robots.txt

`robots.txt` indica a los buscadores qué no indexar. Suele listar ubicaciones que el propietario no desea que se descubran.

### Favicon

El icono puede revelar el framework usado. Consulta la base de datos de OWASP:

- <a href="https://owasp.org/www-community/favicons_database">OWASP_favicon_database</a>

### Sitemap.xml

Lista URLs que el propietario desea indexar, a veces áreas difíciles de navegar o páginas antiguas aún accesibles.

## Cabeceras HTTP

Las respuestas del servidor incluyen cabeceras con información útil como software del servidor o lenguaje de programación. Con `curl -v` puedes verlas y extraer pistas.

## Framework Stack

Una vez identificado el framework (por favicon o pistas en el código), investiga versiones y componentes para localizar posibles vulnerabilidades.
