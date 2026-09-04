# CURSO-PREENTREGAS-N8N
Las entregas estan basadas en una Empresa ficticia creada para este curso, CLEANPRO DISTRIBUCIONES S.A.
# Checkpoint 1: Recepcionista Digital - CleanPro Distribuciones S.A.

## Descripción del Proyecto
Este repositorio contiene el flujo automatizado de la preentrega número 1 para el curso de n8n. El proyecto implementa un "Recepcionista Digital" para la empresa mayorista ficticia **CleanPro Distribuciones S.A.**

## Propósito del Agente
El agente de IA está configurado para:
* Leer y clasificar las consultas entrantes de clientes institucionales mediante un estricto *System Prompt*.
* Derivar las solicitudes de forma operativa hacia los departamentos correspondientes (*Depósito*, *Ventas Mayoristas*, *Administración* o *Escalamiento Humano*) utilizando herramientas de Slack.
* Garantizar la observabilidad y trazabilidad mediante un sistema de control que registra tanto los éxitos como los fallos técnicos.

# Checkpoint 2: Continuación de las Pre entregas
* Manager (Recepcionista Digital): Agente de IA central que clasifica la intención del cliente en una taxonomía cerrada (CONSULTA_STOCK, COTIZACION, RECLAMO, FUERA_DE_ALCANCE) mediante un nodo Switch de rutas deterministas.
* Worker 1 (Stock): Subflujo independiente que consulta disponibilidad y precios directamente desde Google Sheets, retornando un contrato JSON estandarizado.
* Worker 2 (Cotizador): Módulo determinista de cálculo de presupuestos (con lógica en JavaScript de costo cero en tokens), encargado de computar subtotales, aplicar descuentos automáticos por volumen y generar borradores de email.  Observabilidad y Trazabilidad: Consolidación de todas las ramas de negocio mediante un nodo Merge para el registro automático de logs operativos en tiempo real en Slack (#logs-cleanpro).  
