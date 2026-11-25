# Plantilla de PRD para construir con IA

Una plantilla de **PRD (documento de requisitos de producto)** en Markdown, pensada para dársela a un agente de IA como Claude Code, Cursor o Lovable antes de que empiece a programar.

Un agente no pregunta lo que no sabe: lo rellena con la opción más probable. Si el documento no dice en qué país cobras, elegirá una pasarela cualquiera; si no dice quién puede ver qué datos, dejará las tablas abiertas. Cuanto más concreto es el PRD, menos decisiones inventa la IA por ti.

## Qué incluye

| Archivo | Para qué sirve |
|---|---|
| [`PRD.plantilla.md`](PRD.plantilla.md) | La plantilla vacía, con una pista en cada sección |
| [`ejemplos/marketplace-patinetes-galicia.md`](ejemplos/marketplace-patinetes-galicia.md) | Un PRD completo y real, para ver cómo queda rellena |

## Cómo usarla

1. Copia `PRD.plantilla.md` a la raíz de tu proyecto como `PRD.md`.
2. Rellénala en este orden: **objetivo → lo que queda fuera → supuestos → historias con criterios → tablas → tecnología**. Piensa en tablas y en tecnología al final, no al principio.
3. Dile a tu agente que la lea antes de cada tarea:
   - **Claude Code:** añade a `CLAUDE.md` «La especificación del producto está en PRD.md; léela antes de cada tarea».
   - **Cursor:** crea una regla del proyecto que apunte a `PRD.md`, o menciónalo con `@PRD.md`.
   - **Lovable y similares:** pégala en el primer mensaje y pide que la use como especificación.
4. Pide el trabajo **fase a fase** y, al cerrar cada una, que compruebe los criterios de aceptación de sus historias.
5. Si una decisión cambia, **actualiza primero el PRD** y después el código.

## Las secciones, en una línea

- **Objetivo:** qué se construye, en una frase con verbo.
- **Alcance del MVP:** lo que entra y, sobre todo, lo que se deja fuera.
- **Supuestos:** todo lo que diste por hecho (países, moneda, comisiones), para poder discutirlo.
- **Historias y criterios de aceptación:** qué quiere cada rol y cómo se comprueba que lo tiene.
- **Base de datos:** tablas, columnas y reglas de acceso.
- **Pantallas, flujos y seguridad:** las rutas, los flujos delicados (el pago, el registro) y los requisitos propios del producto.
- **Plan por fases y métricas:** en qué orden se construye y cómo sabrás que funciona.

## ¿No quieres rellenarla a mano?

[Generador PRD](https://generadorprd.com) crea un borrador con todas estas secciones a partir de unas líneas de descripción. Es gratis y no necesita registro. Trátalo como un borrador: léelo, confirma los supuestos y corrige lo que no encaje con tu negocio.

Guías relacionadas:

- [Qué es un PRD y qué secciones necesita](https://generadorprd.com/guias/que-es-un-prd)
- [Cómo usar un PRD con un agente de IA](https://generadorprd.com/guias/como-usar-un-prd-con-ia)
- [Biblioteca de PRDs de ejemplo](https://generadorprd.com/biblioteca)

## Licencia

[MIT](LICENSE): úsala, cópiala y adáptala como quieras, también en proyectos comerciales.
