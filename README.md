# AI-AUTOMATION-2
curso Coder Automation
# 🚗 Sistema de Gestión de Siniestros de Automotores

## 🎯 Objetivo del Flujo
Este flujo automatiza el proceso de recepción, análisis y validación de denuncias de siniestros de automotores para una compañía de seguros. Su propósito principal es procesar los reportes de los clientes, verificar la autenticidad de los datos y las pólizas mediante bases de datos externas, evaluar las condiciones del siniestro utilizando Inteligencia Artificial y derivar el caso de manera inteligente hacia un circuito de gestión administrativa automática o hacia una revisión humana (*Human-in-the-Loop*).

---

## 🏗️ Arquitectura y Componentes del Sistema

El flujo está construido en **n8n** utilizando un enfoque modular basado en agentes de IA y sub-workflows. A continuación se detallan sus componentes principales:

1. **Disparador (Trigger)**:
   - **Pipedrive Trigger**: Inicia el flujo automáticamente ante la creación o actualización de un trato/registro relacionado.

2. **Procesamiento y Normalización de Datos**:
   - **Edit Field (Organizador de Datos)**: Extrae y limpia la información clave del mensaje del cliente (como nombre, fecha del siniestro y presupuesto estimado de reparación mediante expresiones regulares).

3. **Verificación de Identidad y Póliza (Airtable)**:
   - **Busca cliente**: Consulta la tabla *Asegurados* en Airtable para validar que el cliente exista y que la póliza se encuentre vigente.
   - **Actualiza datos de clientes**: Actúa como herramienta del Agente de IA para actualizar datos de contacto en Airtable si el asegurado informa cambios verificados.

4. **Evaluación Inteligente (Agente de IA)**:
   - **OpenAI Chat Model (`gpt-5-mini`)** y **Simple Memory**: Un Agente de IA analiza el relato del siniestro y la documentación. Evalúa criterios de riesgo (como montos de presupuesto superiores a $2.000.000 ARS, inconsistencias o indicios de fraude).

5. **Enrutamiento Condicional (Filtros)**:
   - **Requiere Humano o Gestión Automática?**: Nodo condicional que divide el flujo según la resolución del Agente:
     - **Camino Superior (Revisión Humana)**: Deriva el caso al sub-workflow `1_Worker_1_atencion_humana` y prepara alertas o borradores para la intervención de un asesor.
     - **Camino Inferior (Gestión Administrativa)**: Deriva el caso al sub-workflow `2 Worker Gestion Administrativa automatica` para procesar la aprobación o rechazo del siniestro.

6. **Notificaciones y Cierre**:
   - **RECHAZO O APROBADO**: Evalúa el estado final de la resolución.
   - **Gmail**: Envía notificaciones automáticas en formato HTML al asegurado informando el estado de su solicitud (*APROBADO* o *RECHAZADO*).
