# Checklist general para incidencias Mainframe

## 1. Identificación

- ID y nombre del job:
- Fecha y hora de detección:
- ODATE (fecha operativa):
- Entorno afectado:
- Estado actual del job:
- Código de retorno o abend:

## 2. Evaluación del impacto

- ¿Se frena una liquidación o proceso crítico?
- ¿Qué procesos posteriores dependen de este job?
- ¿Hay archivos o entregas pendientes?
- ¿Cuál es la prioridad asignada por el procedimiento del equipo?
- ¿Existe riesgo de duplicar operaciones si se reejecuta?

## 3. Revisión de evidencias

- Revisar JESMSGLG.
- Revisar JESJCL.
- Revisar JESYSMSG.
- Revisar SYSOUT y los logs de la aplicación.
- Identificar el paso del JCL que falló.
- Registrar el código de retorno o abend y el mensaje asociado.

## 4. Historial y dependencias

- ¿El incidente ocurrió anteriormente?
- ¿El job predecesor terminó correctamente?
- ¿Se cumplieron las condiciones de entrada y salida?
- ¿Llegaron los archivos requeridos?
- ¿La ODATE y las fechas de los archivos son correctas?

## 5. Decisión y acción

Antes de actuar, confirmar:

- ¿La causa está identificada o solo se sospecha?
- ¿El procedimiento autoriza un rerun?
- ¿La reejecución puede duplicar operaciones?
- ¿Se necesita corregir JCL, solicitar un archivo o escalar?
- ¿Se requiere autorización de Desarrollo, DBA u Operaciones?

No ejecutar un rerun a ciegas.

## 6. Documentación y comunicación

- Registrar la incidencia en el sistema de tickets.
- Adjuntar mensajes y evidencias relevantes.
- Documentar las verificaciones realizadas.
- Indicar la acción tomada y su resultado.
- Comunicar el estado al equipo de guardia.
- Registrar pendientes y responsable del seguimiento.

## Cierre

- [ ] Causa identificada o hipótesis documentada.
- [ ] Impacto evaluado.
- [ ] Acción validada.
- [ ] Resultado comprobado.
- [ ] Ticket actualizado.
- [ ] Equipo informado.
