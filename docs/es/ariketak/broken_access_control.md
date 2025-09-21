# Broken Access Control - Retos

La mayoría de los sistemas informáticos están diseñados para ser usados por varios usuarios. Los privilegios definen lo que un usuario puede hacer. Entre los privilegios comunes están ver y editar archivos, o modificar archivos del sistema.

La elevación de privilegios significa que un usuario obtiene privilegios que no le corresponden. Con ellos podría borrar archivos, ver información privada o instalar programas no deseados (por ejemplo, virus). Suele ocurrir cuando existe un fallo que permite eludir las medidas de seguridad del sistema o cuando hay suposiciones de diseño equivocadas sobre su uso.

## Retos

### 1. Reto: Miscellaneous > Score Board
    Find the carefully hidden 'Score Board' page.

    Este será nuestro primer ejercicio, ya que en la sección Score Board encontraremos el resto de retos.

### 2. Reto: What's the Administrator's email address?
    Intentaremos encontrar el correo electrónico del usuario administrador.

### 3. Reto: What parameter is used for searching? 
    Al realizar una búsqueda, ¿qué parámetro se utiliza en la consulta?

### 4. Reto: Access the administration section of the store

    Igual que con el Score Board, no tenemos un enlace directo a la sección de administración; tendremos que encontrarla. Hay varias formas posibles de localizarla.

    Para ello, conviene revisar un poco de SQL injection.




## Enlaces
- [OWASP Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)


---

[← Volver al OWASP Top 10](../oinarriak_mehatxuak/owasp_top10.md)