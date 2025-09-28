# Cryptographic Failures

Primero, es necesario definir las necesidades de protección de los datos en tránsito y en reposo. Por ejemplo, las contraseñas, números de tarjetas de crédito, historiales médicos, información personal y secretos comerciales requieren protección adicional, especialmente si están cubiertos por leyes de privacidad.

Por todo lo anterior, verifica:

- ¿Se transmite algún dato en texto claro? Esto afecta a protocolos como HTTP, SMTP o FTP. El tráfico externo en Internet es muy arriesgado. Revisa también el tráfico interno (entre balanceadores, servidores web o sistemas de back-end).

- ¿Se están utilizando algoritmos o protocolos criptográficos obsoletos o débiles, ya sea por defecto o en código heredado?

- ¿Se usan claves criptográficas por defecto, generadas de forma insegura o reutilizadas? ¿Faltan una gestión y rotación adecuadas de claves? ¿Aparecen claves en repositorios de código?

- ¿No se fuerza el cifrado, por ejemplo, por ausencia de cabeceras/directivas de seguridad (del navegador) adecuadas?

- ¿Se validan correctamente los certificados del servidor y la cadena de confianza?

- Etc.

## Retos

### 1. Reto: Confidential Document
Access a confidential document.
Intentaremos localizar un documento confidencial. Para ello conviene revisar secciones como privacy, about us y similares en la aplicación.

### 2. Reto: Sensitive Data Exposure > Exposes credentials
A developer was careless with hardcoding unused, but still valid credentials for a testing account on the client-side.



## Enlaces
- [OWASP Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)


---

[← Volver al OWASP Top 10](../oinarriak_mehatxuak/owasp_top10.md)
