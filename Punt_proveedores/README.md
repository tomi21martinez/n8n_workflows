# Chat de Proveedores (DEV)

**Última actualización:** 2025-10-06  
**Estado:** DEV – Este proyecto se encuentra en fase de desarrollo. No utilizar en producción.

---

## 🧩 Descripción general

El **Chat de Proveedores** es un sistema conversacional diseñado para **centralizar la comunicación con los proveedores de Punt**, automatizando consultas, intercambio de información y procesos internos relacionados con pedidos, catálogos y disponibilidad de productos.

El objetivo principal es **reducir la intervención manual** en la gestión de proveedores, permitiendo un flujo de comunicación fluido, trazable y conectado directamente con los sistemas internos y las APIs de terceros.

Este proyecto está estructurado en dos componentes principales:
- `Chat_proveedores.json` → flujo conversacional principal  
- `Conexion_APIs.json` → conectores e integraciones con APIs externas

---

## ⚙️ Arquitectura y funcionamiento

El sistema está desarrollado sobre una **plataforma de automatización tipo n8n/Make**, basada en flujos JSON que orquestan distintos módulos:

### 🔸 1. Chat_proveedores
Define la **lógica conversacional**, incluyendo:
- Recepción y procesamiento de mensajes de proveedores  
- Identificación de intención y enrutamiento según tipo de consulta  
- Control de contexto de conversación (proveedor, pedido, estado)  
- Generación de respuestas automáticas en lenguaje natural  
- Interacción con los módulos de API mediante peticiones HTTP  

Este flujo actúa como **punto de entrada** del sistema y coordina las operaciones entre los distintos servicios conectados.

---

### 🔸 2. Conexion_APIs
Contiene los **módulos de conexión y datos**, que abstraen la comunicación con sistemas externos, tales como:
- **APIs de proveedores** (catálogo, pedidos, stocks, tarifas)  
- **Sistemas internos** de gestión o ERP (Odoo, CRM, bases SQL, etc.)  
- **Servicios auxiliares** (autenticación, logs, notificaciones, validaciones)

Estas integraciones se gestionan mediante nodos de autenticación, peticiones REST y transformaciones de datos intermedias.  
El objetivo es permitir la **reutilización modular** de los conectores en distintos flujos.

---

## 💬 Flujo general de comunicación

1. **Entrada del mensaje:** el proveedor envía una consulta (por chat, formulario o API).  
2. **Procesamiento:** el flujo identifica el tipo de solicitud y extrae los datos relevantes.  
3. **Integración:** se consulta la API correspondiente (proveedor, catálogo, pedido).  
4. **Respuesta:** el sistema genera una respuesta estructurada o conversacional y la devuelve al canal de origen.  
5. **Registro:** se almacena el intercambio para trazabilidad y análisis posterior.

Este proceso está diseñado para ser **asíncrono, extensible y seguro**, con separación entre la lógica de conversación y las credenciales o configuraciones de API.

---

## 🧠 Tecnologías y principios utilizados

| Componente | Descripción |
|-------------|--------------|
| **n8n / Make** | Plataforma de orquestación de flujos (workflows) en formato JSON. |
| **APIs REST** | Comunicación estándar entre servicios (proveedores, catálogos, pedidos). |
| **HTTP + JSON Schema** | Formato de intercambio y validación de datos. |
| **PostgreSQL** | Base de datos utilizada para almacenamiento de estado, logs y persistencia de datos intermedios. |
| **Control de estado** | Mantenimiento de contexto conversacional por sesión o proveedor. |
| **Logs estructurados** | Registro de ejecución para análisis y debugging. |
| **Autenticación segura** | Uso de tokens y claves API gestionadas fuera del código. |
| **Principio de modularidad** | Separación entre lógica conversacional y conexiones externas. |

---

## 📦 Contenido del repositorio

### `Chat_proveedores/`
```
Chat_proveedores.json
```
Workflow principal del chat.  
Implementa el flujo de conversación, enrutamiento y llamadas a los módulos de conexión.  
Versión **DEV**, sin credenciales ni datos sensibles.

### `Conexion_APIs/`
```
Conexion_APIs.json
```
Conjunto de módulos de integración con APIs externas (proveedores, catálogos, pedidos).  
Versión **DEV**, sin tokens ni información confidencial.

---

## 🔐 Seguridad y manejo de credenciales

Todas las credenciales, tokens y claves se eliminan deliberadamente de los archivos incluidos.  
Para ejecutar los flujos en un entorno real, las credenciales deben configurarse mediante:
- El **gestor de credenciales de la plataforma** (n8n, Make, etc.)
- O variables de entorno (`.env`) seguras

Ejemplo de configuración esperada:
```
PROVEEDOR_API_BASE_URL=https://api.proveedor.com
PROVEEDOR_API_KEY=<tu_clave_aqui>
DATABASE_URL=postgresql://usuario:contraseña@host:puerto/basededatos
PUBLIC_WEBHOOK_URL=https://tuservidor.dev/webhook/proveedores
```

---

## 🧱 Estado de desarrollo

| Módulo | Estado | Descripción |
|--------|---------|-------------|
| Chat_proveedores | 🟡 En pruebas | Flujo funcional, pendiente de validación con datos reales |
| Conexion_APIs | 🟢 Base estable | Estructura modular lista para expansión |
| PostgreSQL | 🟢 Operativo | Almacenamiento de contexto, logs y datos temporales |
| Logging / métricas | 🔴 Pendiente | Integración de sistema de monitorización |
| Documentación técnica | 🟢 Incluida | README y estructura de flujos documentada |

---

## 🧭 Roadmap previsto

- [ ] Añadir sistema de logging estructurado con correlación por sesión  
- [ ] Mejorar manejo de errores y reintentos automáticos  
- [ ] Integración con el sistema de autenticación de Odoo  
- [ ] Añadir métricas de uso y rendimiento  
- [ ] Conectar el flujo con un chatbot front-end para interacción directa  

---

## 🧑‍💻 Autores y mantenimiento

Proyecto desarrollado por el **Departamento de Innovación de Punt Sistemes**.  
Responsable técnico: equipo de integración y automatización.  

Para incidencias o sugerencias, abrir un *issue* interno o contactar con el equipo de innovación.
