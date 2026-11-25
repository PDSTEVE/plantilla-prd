# PRD: Marketplace de patinetes eléctricos de segunda mano en Galicia

> una app que me permita buscar patinetas eléctricas (de 2da baratas) o scooters en buen estado en galicia

## Cómo usar este PRD

Instrucciones para quien lo implemente (persona o agente de IA como Cursor, Claude Code o Lovable):

1. Lee el PRD completo antes de escribir código. Si un supuesto no encaja con lo que sabes del proyecto, pregunta antes de seguir.
2. Construye por fases, en el orden del plan de implementación. Al terminar cada fase, comprueba los criterios de aceptación de las historias que cubre.
3. No añadas nada que esté fuera del alcance del MVP.
4. Respeta el esquema de base de datos y las reglas de datos; si necesitas cambiarlos, explica por qué.
5. Nunca escribas secretos en el código: usa las variables de entorno listadas.

## Resumen

**Objetivo:** Conectar compradores con vendedores particulares de patinetes eléctricos de segunda mano en Galicia, con búsqueda por ciudad y precio.

### Funcionalidades core

- Búsqueda por filtros
- Publicación de anuncios
- Fichas con fotos
- Mensajería interna
- Favoritos
- Geolocalización por ciudad
- Moderación de anuncios
- Reportes

## Alcance del MVP

### Incluye

- Registro e inicio de sesión con email y contraseña
- Publicación de anuncios con hasta 6 fotos
- Búsqueda con filtros de ciudad, precio, marca y estado de batería
- Ficha pública de anuncio con URL amigable
- Favoritos y mensajería interna entre comprador y vendedor
- Reporte de anuncios y panel básico de moderación
- Panel de vendedor para gestionar sus anuncios

### Fuera del MVP

- Pagos y reservas dentro de la app
- Envíos y logística
- Verificación de identidad con documento
- App móvil nativa
- Valoraciones de vendedores
- Alertas push y notificaciones por WhatsApp
- Vendedores profesionales con planes de pago

### Supuestos y decisiones pendientes

- Operación solo en España, en la comunidad autónoma de Galicia (A Coruña, Lugo, Ourense y Pontevedra).
- Precios en euros, guardados en céntimos como enteros.
- La transacción se cierra en persona; la app no cobra comisión en el MVP.
- Los anuncios se publican al momento y el administrador los revisa a posteriori tras un reporte.
- Idioma principal español; gallego opcional en una fase posterior.
- Solo vendedores particulares, sin tiendas profesionales en el MVP.
- Cada usuario puede tener un máximo de 20 anuncios activos.

## Stack tecnológico

- **Frontend:** Next.js (React)
- **Backend:** Supabase (Auth, PostgreSQL, Storage)
- **APIs externas:** Mapbox (mapa y geocodificación de ciudades), Resend (email transaccional), Cloudflare Turnstile (protección contra bots)

## Historias de usuario y criterios de aceptación

1. Como comprador, quiero buscar patinetes por ciudad, precio y estado de batería para encontrar opciones cerca de mí sin revisar anuncios uno a uno.
   - [ ] Al filtrar por ciudad y precio máximo, solo aparecen anuncios activos de esas condiciones ordenados por fecha de publicación.
   - [ ] Si no hay resultados, la página muestra un mensaje y un botón para limpiar filtros.
   - [ ] Los filtros se mantienen en la URL al recargar la página.
2. Como comprador, quiero ver fotos, estado de batería, kilometraje y precio de cada anuncio para decidir si me interesa.
   - [ ] La ficha muestra fotos, marca, modelo, batería, kilometraje, precio y ciudad del anuncio.
   - [ ] Si el anuncio está marcado como vendido, la ficha muestra la etiqueta Vendido y oculta el botón de contacto.
   - [ ] Si el anuncio no existe, se muestra un error 404.
3. Como comprador, quiero guardar anuncios en favoritos para compararlos más tarde.
   - [ ] Al pulsar el icono de favorito, el anuncio aparece en /favoritos tras recargar la página.
   - [ ] Un usuario no autenticado que pulsa favorito es redirigido a /login.
   - [ ] Quitar un favorito lo elimina de la tabla favorites sin borrar el anuncio.
4. Como comprador, quiero escribir al vendedor desde el anuncio para preguntar detalles o cerrar una visita.
   - [ ] Al enviar el primer mensaje se crea una fila en conversations y otra en messages.
   - [ ] El vendedor recibe un email con Resend en menos de 5 minutos.
   - [ ] Un usuario no puede escribir en una conversación en la que no es comprador ni vendedor.
5. Como vendedor, quiero publicar mi patinete con fotos y datos técnicos para venderlo a compradores de Galicia.
   - [ ] El formulario exige marca, modelo, estado, batería, precio y ciudad antes de publicar.
   - [ ] Al publicar se guardan el anuncio y sus imágenes en Storage con posición ordenada.
   - [ ] Si la subida de una imagen falla, el anuncio no se publica y el usuario ve el error.
6. Como vendedor, quiero marcar mi anuncio como vendido o pausarlo para que deje de aparecer en las búsquedas.
   - [ ] Al marcar como vendido, el estado cambia a sold y el anuncio deja de aparecer en la búsqueda.
   - [ ] Al pausar, el estado cambia a paused y solo el vendedor lo ve en su panel.
   - [ ] Un vendedor no puede cambiar anuncios de otro usuario; la base de datos lo rechaza.
7. Como administrador, quiero revisar anuncios reportados y aprobar o retirarlos para mantener la calidad del catálogo.
   - [ ] El administrador ve en /admin/reportes los reportes pendientes con enlace al anuncio.
   - [ ] Al retirar un anuncio, su estado pasa a removed y el reporte pasa a resolved.
   - [ ] Un usuario que no es administrador recibe error 403 en /admin.

## Base de datos

- `profiles`: `id`, `display_name`, `city`, `province`, `created_at`
- `cities`: `id`, `name`, `province`, `lat`, `lng`
- `listings`: `id`, `seller_id`, `city_id`, `slug`, `brand`, `model`, `condition`, `battery_health_pct`, `mileage_km`, `price_cents`, `description`, `status`, `created_at`, `updated_at`
- `listing_images`: `id`, `listing_id`, `storage_path`, `position`
- `favorites`: `id`, `user_id`, `listing_id`, `created_at`
- `conversations`: `id`, `listing_id`, `buyer_id`, `seller_id`, `created_at`
- `messages`: `id`, `conversation_id`, `sender_id`, `body`, `created_at`
- `reports`: `id`, `listing_id`, `reporter_id`, `reason`, `status`, `created_at`

### Reglas de datos y permisos

- listings.price_cents es un entero positivo en céntimos de euro.
- favorites: único por (user_id, listing_id).
- conversations: único por (listing_id, buyer_id).
- listings.status solo admite draft, active, sold, paused y removed.
- reports.status solo admite pending, reviewed y resolved.
- Los borrados de anuncios son lógicos (status removed) para conservar el historial de mensajes.
- RLS: cualquiera puede leer listings activos; solo el vendedor lee y escribe sus borradores y anuncios pausados.
- RLS: messages solo son legibles por el comprador y el vendedor de la conversación.
- RLS: profiles solo puede editarlo el propio usuario; los administradores leen reports.

## Pantallas y rutas

- / — Portada con buscador rápido y anuncios recientes; la usan todos los visitantes.
- /buscar — Listado con filtros de ciudad, precio, marca y batería; lo usan compradores.
- /anuncios/[slug] — Ficha del anuncio con fotos, datos técnicos y botón de contacto; la usan compradores.
- /publicar — Formulario de alta de anuncio con subida de fotos; lo usan vendedores.
- /mis-anuncios — Panel del vendedor para editar, pausar o marcar como vendido; lo usan vendedores.
- /favoritos — Lista de anuncios guardados; la usan compradores.
- /mensajes — Bandeja de conversaciones; la usan compradores y vendedores.
- /mensajes/[id] — Chat de una conversación concreta; lo usan compradores y vendedores.
- /perfil — Datos del usuario y ciudad; lo usan todos los usuarios registrados.
- /login — Inicio de sesión y registro; lo usan visitantes.
- /admin/reportes — Cola de reportes pendientes con acciones; la usa el administrador.
- /admin/anuncios — Listado de anuncios con filtro por estado para moderar; lo usa el administrador.

## Flujos clave

### Publicar anuncio

1. El vendedor completa el formulario en /publicar y pulsa publicar.
2. Turnstile valida que la petición la hace una persona.
3. El frontend sube las fotos a Supabase Storage en el bucket listings.
4. Se crea la fila en listings con status draft y las filas en listing_images.
5. Se cambia el status a active en una sola transacción de servidor.
6. Si falla la subida de alguna foto, se borran las ya subidas y el anuncio queda en draft con aviso.
7. El anuncio aparece en /buscar y en /mis-anuncios.

### Contactar vendedor

1. El comprador pulsa contactar en la ficha del anuncio.
2. Si no tiene sesión, se le redirige a /login y vuelve a la ficha.
3. Se crea la conversación en conversations o se recupera la existente.
4. Se inserta el mensaje en messages.
5. Un webhook de Supabase llama a una función que envía el email al vendedor con Resend.
6. Si Resend falla, el mensaje queda guardado y se reintenta tres veces con espera creciente.
7. El vendedor ve el mensaje en /mensajes al entrar.

### Moderar reporte

1. Un usuario pulsa reportar en la ficha y elige un motivo.
2. Se inserta una fila en reports con status pending.
3. El administrador abre /admin/reportes y ve el anuncio y el motivo.
4. Si retira el anuncio, se cambia su status a removed.
5. Se marca el reporte como resolved.
6. El vendedor recibe un email de aviso con Resend.
7. Si el anuncio ya estaba removed, la acción se bloquea y muestra aviso.

## Seguridad y privacidad

- Las políticas RLS de Supabase deben estar activas en todas las tablas antes de publicar.
- Las fotos se sirven con URLs firmadas de corta duración; el bucket no es público.
- Validar en servidor que el usuario es el propietario antes de cambiar el estado de un anuncio.
- Turnstile protege el registro, la publicación y el envío de mensajes.
- Limitar la subida a imágenes JPEG, PNG o WebP de máximo 5 MB.
- No mostrar el teléfono del vendedor en la ficha; el contacto va por mensajería interna.

## Plan de implementación

### Fase 1: Base del proyecto

- [ ] Crear proyecto Next.js y proyecto Supabase.
- [ ] Configurar Auth con email y contraseña.
- [ ] Crear el esquema de tablas y las migraciones.
- [ ] Activar RLS y escribir las políticas básicas.
- [ ] Cargar la tabla cities con los municipios de Galicia.

### Fase 2: Catálogo y búsqueda

- [ ] Implementar el formulario de publicación con subida de fotos.
- [ ] Crear la ficha pública con URL amigable.
- [ ] Construir /buscar con filtros que se guardan en la URL.
- [ ] Integrar Mapbox para seleccionar la ciudad.

### Fase 3: Comunicación y favoritos

- [ ] Implementar favoritos en /favoritos.
- [ ] Crear la mensajería entre comprador y vendedor.
- [ ] Conectar el webhook y el envío de emails con Resend.
- [ ] Añadir el panel de vendedor con cambios de estado.

### Fase 4: Moderación y protección

- [ ] Implementar el flujo de reportes.
- [ ] Crear el panel /admin con cola de reportes.
- [ ] Integrar Cloudflare Turnstile en registro, publicación y mensajes.
- [ ] Añadir límites de subida y de anuncios activos.

### Fase 5: Pruebas y lanzamiento

- [ ] Probar los tres flujos críticos de extremo a extremo.
- [ ] Revisar políticas RLS con usuarios de prueba.
- [ ] Publicar en Vercel y configurar el dominio.
- [ ] Preparar la página de términos y privacidad.

## Variables de entorno

- NEXT_PUBLIC_SUPABASE_URL — URL pública del proyecto Supabase
- NEXT_PUBLIC_SUPABASE_ANON_KEY — clave anónima para el cliente con RLS
- SUPABASE_SERVICE_ROLE_KEY — clave de servidor para funciones y webhooks
- NEXT_PUBLIC_MAPBOX_TOKEN — token público para el mapa y la geocodificación
- RESEND_API_KEY — clave para enviar emails transaccionales
- NEXT_PUBLIC_TURNSTILE_SITE_KEY — clave pública de Cloudflare Turnstile
- TURNSTILE_SECRET_KEY — clave secreta para validar Turnstile en servidor
- SUPABASE_WEBHOOK_SECRET — secreto para verificar los webhooks de mensajes

## Métricas de éxito

- Anuncios activos publicados en los primeros 3 meses > 300
- Tasa de anuncios con al menos 4 fotos > 60 %
- Conversaciones iniciadas por anuncio activo > 0,4
- Tasa de anuncios marcados como vendidos en 60 días > 25 %
- Reportes resueltos en menos de 48 horas > 90 %

---

Generado con [Generador PRD](https://generadorprd.com/p/una-app-que-me-permita-buscar-patinetas-electricas-3469e11e)
