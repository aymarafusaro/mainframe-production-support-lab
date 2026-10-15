
# Procedimiento de guardia de producción

## Objetivo

Registrar, investigar, comunicar y resolver incidencias de manera controlada.

## Flujo de trabajo

1. **Identificar:** registrar job, entorno, fecha y hora, ODATE y estado.
2. **Evaluar:** determinar qué proceso está afectado y el impacto potencial.
3. **Investigar:** revisar logs, códigos de retorno, dependencias y antecedentes.
4. **Formular una hipótesis:** indicar qué evidencia la respalda y qué falta comprobar.
5. **Decidir:** determinar si corresponde una acción operativa autorizada o un escalamiento.
6. **Validar:** comprobar el resultado de la acción y el estado de los procesos posteriores.
7. **Documentar:** actualizar el ticket y comunicar el resultado.

## Criterios de escalamiento

* Desarrollo: defectos de lógica COBOL, datos incompatibles con el programa o errores de aplicación.
* DBA: recursos DB2, packages, locks y errores que requieren intervención sobre la base.
* Infraestructura: indisponibilidad de recursos, problemas de entorno o almacenamiento.
* Operaciones: dependencias, planificación y procedimientos de ejecución.
* Equipo de origen: archivos faltantes, vacíos o incorrectos.

La asignación definitiva depende de la organización y sus procedimientos.

## Datos mínimos para comunicar una incidencia

* Job y paso afectado.
* Fecha, hora y ODATE.
* Código de retorno, abend o SQLCODE.
* Mensajes relevantes.
* Impacto confirmado o pendiente de determinar.
* Verificaciones realizadas.
* Acción tomada y resultado.
* Equipo al que se escaló.
* Próximo paso y responsable.

## Cierre de la incidencia

Cerrar únicamente cuando el resultado esté verificado según los criterios del proceso y el ticket contenga la evidencia necesaria. Si la causa no está resuelta, dejar explícitos el estado, el riesgo y el seguimiento pendiente.
