# Chat de Proveedores (DEV) – README

_Última actualización: 2025-10-06_

> **Estado:** DEV · Estos flujos aún están en desarrollo y pueden cambiar sin previo aviso. No usar en producción.

## 📦 Contenido del repositorio
### `Chat_proveedores/`
```

```
_No se han detectado JSONs en este directorio._

### `Conexion_APIs/`
```

```
_No se han detectado JSONs en este directorio._

## 🧩 Propósito y arquitectura (resumen)
Este conjunto de workflows orquesta un **chat de proveedores** para Punt, con automatizaciones que conectan el front/UX conversacional con **servicios y APIs** (ver `Conexion_APIs/`). A alto nivel:

- **Ingesta y enrutado**: recepción de mensajes, normalización y derivación a intents.
- **Conectores y datos** (`Conexion_APIs`): módulos que encapsulan llamadas a APIs externas (proveedores, catálogos, estado de pedidos, autenticación, etc.).
- **Gestión de contexto**: almacenamiento de estado de conversación (sesión, proveedor, pedido) y recuperación de contexto.
- **Reglas de negocio**: validaciones, políticas de visibilidad, formatos de respuesta y control de errores.
- **Observabilidad**: logs de ejecución y métricas básicas para diagnóstico durante DEV.

## ⚙️ Requisitos previos
- Node 18+ o Docker (si se usa n8n/Make u otra plataforma de orquestación, ajustar según entorno).
- Accesos y credenciales a las APIs de proveedores (ver **Variables de entorno**).
- Base de datos/almacenamiento para estado (p. ej., PostgreSQL/Redis) si aplica al orquestador.
- URL de callback o webhook pública (ngrok o túnel equivalente) para pruebas locales.

## 🔐 Variables de entorno y secretos
Crea un archivo `.env` (no lo subas al repo) con al menos:

```
# Identidad app
APP_ENV=dev
APP_NAME=chat_proveedores_dev

# Proveedores (ejemplos)
PROVEEDOR_API_BASE_URL=
PROVEEDOR_API_KEY=

# Autenticación / OAuth (si aplica)
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRET=
OAUTH_TOKEN_URL=

# Almacenamiento (si aplica)
DATABASE_URL=
REDIS_URL=

# Webhooks
PUBLIC_WEBHOOK_URL=
WEBHOOK_SIGNATURE_SECRET=

# Observabilidad
LOG_LEVEL=debug
```

> Revisa los JSONs en `Conexion_APIs/` para añadir claves específicas de cada conector.

## 🚀 Puesta en marcha (DEV)
1. **Clona** el repo y copia estos directorios en tu entorno de orquestación (n8n/Make/u otro).
2. **Importa** los JSONs de `Conexion_APIs` (conectores) y luego los de `Chat_proveedores` (flujos conversacionales).
3. **Configura credenciales** en el panel del orquestador con las variables del `.env`.
4. **Ajusta endpoints** de webhooks/HTTP Request dentro de los flujos para que apunten a tu `PUBLIC_WEBHOOK_URL`.
5. **Activa** los workflows **en sandbox** (nunca en producción).
6. **Prueba** con casos controlados (mensajes de ejemplo, proveedores mock).

> Si un JSON no importa por dependencias, importa primero los conectores base de `Conexion_APIs/`.

## 🧪 Testing sugerido
- **Happy path**: consulta de catálogo, disponibilidad y precios para un proveedor conocido.
- **Errores controlados**: API 4xx/5xx, timeouts y reintentos con backoff.
- **Datos faltantes**: mensajes incompletos o sin contexto de pedido/proveedor.
- **Seguridad**: validación de firmas de webhook y sanitización de entradas.
- **Idempotencia**: reenvío de eventos duplicados.
- **Internacionalización** (si aplica): formatos de moneda/fecha.

## 📡 Endpoints y triggers (orientativo)
- **/webhook/proveedores**: entrada de mensajes del chat.
- **/api/proveedores/:id**: detalle/estado de proveedor.
- **/api/pedidos/:id**: estado de pedido y líneas.
- **Tareas programadas**: sincronización periódica de catálogos/estados (cron).
> Ajusta a los endpoints reales definidos en los JSONs importados.

## 🧯 Troubleshooting
- **Import error / credenciales**: verifica que el nombre del credencial en el orquestador coincide con el usado en el JSON.
- **CORS / webhooks**: usa un túnel (ngrok) y permite la URL exacta en la configuración de la API del proveedor.
- **429 / rate limits**: habilita colas y límites de concurrencia por conector.
- **Campos inesperados**: revisa los mapeos en nodos transformadores antes/después de llamadas HTTP.
- **UTF-8 y acentos**: confirma codificación en nodos/plantillas.

## 🗺️ Roadmap (DEV)
- [ ] Catálogo de proveedores unificado y caché.
- [ ] Reintentos con política exponencial por conector.
- [ ] Métricas y trazas (OpenTelemetry / logs estructurados).
- [ ] Tests de contrato para APIs externas.
- [ ] Plantillas de respuesta multimodal (texto + adjuntos).

## 🤝 Contribución
- Haz PRs pequeños y enfocados.
- Incluye **casos de prueba** y **notas de migración** si el cambio afecta a datos.
- Usa `feat:`, `fix:`, `chore:`, `docs:` en los mensajes de commit.

## 📝 Licencia y datos
- Uso interno de Punt, **solo entorno DEV**.
- No incluir datos sensibles de clientes/proveedores en los ejemplos.

## 📞 Contacto
- Equipo de Innovación (Punt) – responsable del chat de proveedores.
- Incidencias: abrir ticket en el repositorio interno.