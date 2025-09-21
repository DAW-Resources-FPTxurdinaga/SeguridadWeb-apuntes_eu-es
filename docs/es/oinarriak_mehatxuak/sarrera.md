# 1.1 Introducción a la Ciberseguridad en el Desarrollo Web

## ¿Por qué es importante la ciberseguridad en el desarrollo web?

Las aplicaciones web funcionan hoy como repositorios de datos sensibles de empresas y usuarios. Implementar ciberseguridad ayuda a proteger:

- La confidencialidad de los datos privados de los usuarios
- La integridad y disponibilidad del sitio web
- La reputación y la confianza en la organización
- El cumplimiento legal (GDPR, CCPA, etc.)

## El rol del desarrollador y el enfoque "Shift Left"

Tradicionalmente, la seguridad se consideraba al final del proceso de desarrollo. El enfoque "Shift Left" propone integrar la seguridad desde el inicio del desarrollo:

- Prevenir de forma anticipada: corregir los fallos antes de que se introduzcan
- Reducir costes: detectar problemas tempranamente es más barato
- Mejorar la calidad del código: la seguridad pasa a ser parte integral del código

## Triángulo CIA (Confidencialidad, Integridad, Disponibilidad)

Principios básicos de la ciberseguridad:

1. Confidencialidad (Confidentiality)

- Control de acceso adecuado
- Cifrado de datos
- Acceso sólo para personas autorizadas

2. Integridad (Integrity)

- Mecanismos para garantizar la exactitud e integridad de los datos
- Sistemas para detectar cambios no autorizados
- Trazabilidad de transacciones

3. Disponibilidad (Availability)

- Continuidad del servicio
- Capacidad para gestionar ataques y fallos
- Rendimiento adecuado

## Amenaza, vulnerabilidad y riesgo

Comprender amenazas, vulnerabilidades y riesgos es clave:

- Amenaza (Threat): un evento o actor capaz de causar daño al sistema.
- Vulnerabilidad (Vulnerability): un punto débil del sistema que un atacante puede explotar.
- Riesgo (Risk): probabilidad e impacto de que una amenaza explote una vulnerabilidad y cause daño.

## Diseño seguro

### Seguridad por diseño (Security by Design)
La seguridad no es un añadido; debe formar parte del diseño. Para ello:

- Integrar la seguridad desde el inicio del desarrollo
- Aplicar principios básicos de seguridad
- Usar configuraciones seguras por defecto

### Defensa en profundidad (Defense in Depth)
Organizar la protección en múltiples capas, cada una aportando su defensa. Si una capa falla, la siguiente sigue protegiendo para detener o limitar el ataque:

- Implementar múltiples capas de seguridad
- Emplear varios mecanismos defensivos
- Si el atacante supera una capa, que otra pueda detenerlo

### Mínimo privilegio (Least Privilege)
Cada usuario o proceso debe tener sólo los permisos necesarios para realizar su función:

- Conceder los permisos mínimos necesarios a usuarios y procesos
- Restringir privilegios de administrador
- Gestionar activamente los permisos

## Próximos pasos

- [Fundamentos del funcionamiento de la web](web_oinarriak.md)
- [Visión general de vulnerabilidades comunes](owasp_top10.md)
