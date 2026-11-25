# PRD: [Nombre del producto]

> [La idea en una o dos frases, tal como se la contarías a alguien en un ascensor.]

## Cómo usar este PRD

Instrucciones para quien lo implemente (persona o agente de IA como Cursor, Claude Code o Lovable):

1. Lee el PRD completo antes de escribir código. Si un supuesto no encaja con lo que sabes del proyecto, pregunta antes de seguir.
2. Construye por fases, en el orden del plan de implementación. Al terminar cada fase, comprueba los criterios de aceptación de las historias que cubre.
3. No añadas nada que esté fuera del alcance del MVP.
4. Respeta el esquema de base de datos y las reglas de datos; si necesitas cambiarlos, explica por qué.
5. Nunca escribas secretos en el código: usa las variables de entorno listadas.

## Resumen

**Objetivo:** [Una frase que empiece con un verbo: "Permitir que…", "Conectar…", "Automatizar…". Si no cabe en una frase, la idea aún no está clara.]

### Funcionalidades core

- [Funcionalidad 1]
- [Funcionalidad 2]
- [Funcionalidad 3]

## Alcance del MVP

### Incluye

- [Lo mínimo para que el producto sirva de punta a punta]

### Fuera del MVP

- [Lo que se deja fuera a propósito. Es la lista más útil: evita que el proyecto crezca sin control.]

### Supuestos y decisiones pendientes

- [País o países donde opera y moneda]
- [Quién cobra, qué comisión y quién la paga]
- [Idioma]
- [Quién aprueba o modera qué]
- [Cualquier cosa que hayas dado por hecho y alguien podría discutir]

## Stack tecnológico

- **Frontend:** [p. ej. Next.js]
- **Backend y base de datos:** [p. ej. Supabase (Auth, PostgreSQL, Storage)]
- **Servicios externos:** [pagos, email, mapas… elegidos según lo que promete el producto, no por costumbre]

## Historias de usuario y criterios de aceptación

Una historia por rol (cliente, proveedor y administrador). Cada historia lleva 2–3 criterios que se puedan comprobar: nada de «funciona bien» o «es rápido».

### Historia 1

**Como** [rol], **quiero** [acción] **para** [beneficio].

- [ ] [Criterio verificable 1]
- [ ] [Criterio verificable 2]
- [ ] [Criterio verificable 3]

### Historia 2

**Como** [rol], **quiero** [acción] **para** [beneficio].

- [ ] [Criterio verificable 1]
- [ ] [Criterio verificable 2]

## Base de datos

| Tabla | Columnas |
|---|---|
| `users` | `id`, `role`, `name`, `email`, `created_at` |
| `[tabla]` | `[columnas]` |

### Reglas de datos

- [Qué es único, p. ej. un email por usuario]
- [Cómo se guardan los importes: en céntimos como enteros, nunca en decimales]
- [Quién puede leer y quién puede escribir cada tabla (políticas RLS si usas Supabase)]
- [Qué se borra y qué se conserva cuando un usuario elimina su cuenta]

## Pantallas y rutas

| Ruta | Pantalla | Quién accede |
|---|---|---|
| `/` | [Inicio] | Todos |
| `/[ruta]` | [Pantalla] | [Rol] |

## Flujos clave

Solo los flujos donde es fácil equivocarse: el pago, el registro, la aprobación, la cancelación.

### [Nombre del flujo]

1. [Paso 1]
2. [Paso 2]
3. [Qué pasa si falla a mitad]

## Seguridad y privacidad

- [Cada tabla con reglas de acceso antes de publicar]
- [Webhooks de pago: verificar la firma antes de cambiar ningún estado]
- [Importes y comisiones calculados solo en el servidor]
- [Archivos privados con URLs firmadas de vida corta]
- [Datos personales que se guardan y por qué]

## Plan de implementación

### Fase 1: Base

- [ ] Proyecto, autenticación con roles, esquema de base de datos y permisos

### Fase 2: Flujo principal

- [ ] [Tareas del flujo principal, de punta a punta, sin pagos]

### Fase 3: Pagos e integraciones

- [ ] [Pasarela en modo de prueba, webhooks, emails]

### Fase 4: Administración y lanzamiento

- [ ] [Paneles de administración, detalles de lanzamiento]

## Variables de entorno

| Variable | Para qué sirve |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | URL del proyecto de base de datos |
| `[VARIABLE]` | [Descripción] |

## Métricas de éxito

- [Qué número te dirá que el MVP funciona, y con qué valor lo darás por bueno]
