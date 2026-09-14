# CURSO-PREENTREGAS-N8N
Las entregas están basadas en una Empresa ficticia creada para este curso, CLEANPRO DISTRIBUCIONES S.A.

# Checkpoint 1: Recepcionista Digital - CleanPro Distribuciones S.A.

## Descripción del Proyecto
Este repositorio contiene el flujo automatizado de la preentrega número 1 para el curso de n8n. El proyecto implementa un "Recepcionista Digital" para la empresa mayorista ficticia **CleanPro Distribuciones S.A.**

## Propósito del Agente
El agente de IA está configurado para:
* Leer y clasificar las consultas entrantes de clientes institucionales mediante un estricto *System Prompt*.
* Derivar las solicitudes de forma operativa hacia los departamentos correspondientes (*Depósito*, *Ventas Mayoristas*, *Administración* o *Escalamiento Humano*) utilizando herramientas de Slack.
* Garantizar la observabilidad y trazabilidad mediante un sistema de control que registra tanto los éxitos como los fallos técnicos.

# Checkpoint 2: Continuación de las Pre entregas
* **Manager (Recepcionista Digital):** Agente de IA central que clasifica la intención del cliente en una taxonomía cerrada (CONSULTA_STOCK, COTIZACION, RECLAMO, FUERA_DE_ALCANCE) mediante un nodo Switch de rutas deterministas.
* **Worker 1 (Stock):** Subflujo independiente que consulta disponibilidad y precios directamente desde Google Sheets, retornando un contrato JSON estandarizado.
* **Worker 2 (Cotizador):** Módulo determinista de cálculo de presupuestos (con lógica en JavaScript de costo cero en tokens), encargado de computar subtotales, aplicar descuentos automáticos por volumen y generar borradores de email.  
* **Observabilidad y Trazabilidad:** Consolidación de todas las ramas de negocio mediante un nodo Merge para el registro automático de logs operativos en tiempo real en Slack (#logs-cleanpro).  

## Pre-Entrega 3 (Módulo 3): Memoria Persistente y Resumen Automático

En esta iteración, el sistema evolucionó para incorporar memoria a largo plazo y optimización de contexto, resolviendo el problema de la amnesia entre ejecuciones:

* **Persistencia Híbrida en Airtable:** Implementación de un circuito de lectura y escritura (upsert) para guardar el estado del caso y consultar el historial del cliente mediante un `Session_ID` único.
* **Capa de Summarization:** Creación de una ruta condicional que se activa automáticamente al superar los 5 intercambios de mensajes. Utiliza el modelo `gpt-4o-mini` con un prompt estricto para comprimir el historial en un JSON estructurado (Asunto Principal, Puntos Clave, Acción Requerida).
* **Inyección de Contexto (Context Engineering):** El Manager Agent recibe el resumen consolidado entre delimitadores rígidos de protección en su System Prompt, lo que le permite retomar conversaciones de clientes recurrentes optimizando el consumo de tokens.
* **Archivo de este hito:** `manager_modulo3_almeyda_fabiana.json`

## Pre-Entrega 4 (Módulo 4): Integraciones Avanzadas e Interconexión de Sistemas (Arquitectura Manager-Worker M3)

En este hito se extiende el ecosistema agéntico para conectarlo con herramientas reales del negocio (CRM HubSpot, Gmail y Slack) bajo protocolos seguros y principios estrictos de gobernanza, integrándose de forma síncrona con el Manager y la memoria del Módulo 3:

- **Continuidad Modular (`Execute Workflow`):** El workflow actúa como puerta de entrada y brazo ejecutor, delegando el razonamiento y la persistencia de Airtable al Manager del Módulo 3, resolviendo la deuda de flujo independiente.
- **Filtro Anti Auto-Reply:** Nodo `IF` determinista con expresiones regulares robustas para descartar correos automáticos (Auto-reply, Out of office, Undeliverable, no-reply@), neutralizando bucles infinitos.
- **Normalización y Blindaje de Datos (`Set`):** Implementación de funciones seguras en JavaScript con protección defensiva contra valores `undefined` y control de arreglos para extraer de forma limpia el `sessionId`, `cliente_email`, `cliente_nombre` y `mensaje_usuario`.
- **Limpieza y Validación de Payload (Anti Error 400):** Nodos intermedios para validar la estructura del correo y asegurar la integridad de los datos antes de operar con APIs.
- **Look Up en HubSpot y Resiliencia (Anti Error 409):** Búsqueda previa de contactos por correo electrónico para evitar duplicados, configurada con reintentos automáticos (`retryOnFail`, 3 intentos) para tolerar intermitencias de red y credenciales de appToken correctamente mapeadas.
- **Create Draft en Gmail (Human-in-the-Loop):** Configuración estricta de seguridad bajo el principio de mínimo privilegio (`gmail.compose`), generando un borrador oficial para revisión humana previa al envío.
- **Auditoría en Slack:** Envía notificaciones de estado al canal `#logs-cleanpro-agente` consumiendo directamente los datos validados del nodo de limpieza y evitando referencias a nodos vacíos.
- **Archivo de este hito:** `CleanPro · E2E Módulo 4 con Execute Workflow Manager M3.json`
