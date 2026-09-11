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

## Pre-Entrega 4 (Módulo 4): Integraciones Avanzadas e Interconexión de Sistemas

En este hito se extiende el ecosistema agéntico para conectarlo con herramientas reales del negocio (CRM, Gmail y Slack) bajo protocolos seguros OAuth2 y principios estrictos de gobernanza:

* **Filtro Anti Auto-Reply:** Nodo IF determinista con expresiones regulares para descartar correos automáticos (*Auto-reply, Out of office, Undeliverable, no-reply@*), neutralizando bucles infinitos.
* **Look Up en HubSpot (Anti Error 409):** Búsqueda previa de contactos por correo electrónico antes de ejecutar la acción de creación, evitando duplicados en la base de datos de ventas de CleanPro.
* **Create Draft en Gmail (Human-in-the-Loop):** Configuración estricta de seguridad bajo el principio de mínimo privilegio (`gmail.compose`), asegurando que ningún correo salga de forma autónoma sin la aprobación y revisión humana.
* **Limpieza de Payload (Anti Error 400):** Nodos intermedios (`Set`) para validar y limpiar objetos pesados o vacíos antes de notificar al canal de operaciones.
* **Archivo de este hito:** `checkpoint4_almeyda_fabiana.json`
