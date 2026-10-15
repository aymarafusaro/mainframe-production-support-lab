
# Catálogo de errores Mainframe

Guía de referencia para practicar el diagnóstico de incidencias.
Los códigos deben analizarse junto con los mensajes del job, el paso afectado y el contexto de ejecución.

## 1. Control-M

### WAIT CONDITION

**Síntoma:** el job no comienza porque espera una condición.

Verificar:

* Estado del job predecesor.
* Condiciones de entrada y salida.
* Recepción del archivo esperado.
* ODATE y fecha del proceso.

Acción:

* Identificar qué condición falta.
* Si falta un archivo, contactar al equipo de origen.
* Modificar o forzar condiciones únicamente con autorización y siguiendo el procedimiento operativo.

### NOTOK / FAILED

**Síntoma:** la ejecución terminó con error.

Verificar:

* Log del job.
* Código de retorno.
* Paso que falló.
* Mensajes de JES y SYSOUT.

Acción:

* Identificar la causa antes de decidir si corresponde corregir, reejecutar o escalar.

### LONG TIME / ejecución prolongada

**Síntoma:** el job continúa ejecutándose más tiempo del esperado.

Verificar:

* Duración habitual e historial.
* Consumo de recursos, si está disponible.
* Esperas de datasets u otros recursos.
* Posibles problemas de lógica en el programa.

Acción:

* Seguir el procedimiento de cancelación y escalamiento.
* No cancelar ni reejecutar sin evaluar el impacto.

## 2. JCL, datasets y VSAM

### JCL ERROR

**Síntoma:** el JCL no supera la validación o no puede ejecutarse correctamente.

Verificar:

* Sintaxis y nombres de sentencias.
* DD statements y parámetros.
* Referencias a datasets y bibliotecas.
* Mensajes de JESJCL.

Acción:

* Corregir el problema identificado y validar antes de volver a enviar el job.

### S213 / S214

**Síntoma:** posibles problemas de acceso a datasets, según el mensaje concreto.

Verificar:

* Nombre del dataset.
* Existencia y catalogación.
* DISP y permisos.
* Volumen y disponibilidad del recurso.
* Mensaje completo del sistema.

Acción:

* Resolver según la causa confirmada; escalar a Operaciones o Infraestructura si corresponde.

### S0C4

**Síntoma:** excepción de protección o acceso a memoria.

Verificar:

* Programa y paso afectados.
* Mensajes y ubicación de la falla.
* Contexto de ejecución.

Acción:

* Reunir evidencias y escalar a Desarrollo cuando corresponda.

### S322

**Síntoma:** se superó el límite de tiempo de ejecución del job.

Verificar:

* Tiempo habitual de ejecución.
* Paso donde se produjo la terminación.
* Posibles bucles, procesamiento excesivo o esperas.

Acción:

* Investigar la causa y seguir el procedimiento de escalamiento. No reejecutar sin analizar el riesgo de repetición.

## 3. COBOL

### S0C7 — Data Exception

**Síntoma:** una operación encuentra datos incompatibles con el formato numérico esperado.

Verificar:

* Programa y paso afectados.
* Instrucción indicada por las herramientas de diagnóstico disponibles.
* Campo involucrado y contenido del registro.
* Si corresponde, validar el dato en el origen.

Acción:

* Guardar la evidencia y escalar a Desarrollo si requiere modificar el programa o corregir los datos de origen.

### S0CB — División por cero

**Síntoma:** se produce una excepción durante una división.

Verificar:

* Operación y datos involucrados.
* Valor del divisor.
* Mensajes disponibles.

Acción:

* Documentar y escalar para revisar la lógica o los datos.

### S806 / S0C1

**Síntoma:** pueden indicar problemas de carga o ejecución de programas.

Verificar:

* Nombre del programa.
* Existencia de la biblioteca de carga.
* STEPLIB o JOBLIB, si corresponde.
* Mensaje exacto del sistema.

Acción:

* Corregir la referencia únicamente después de confirmar la causa; escalar si se requiere un cambio de despliegue.

## 4. DB2

### SQLCODE -805

Posible problema de localización del package o plan requerido.

Verificar:

* Programa y package.
* Colección y plan.
* Mensaje completo y cambios recientes de despliegue.

Acción:

* Coordinar con Desarrollo o DBA para validar el BIND y la configuración.

### SQLCODE -818

Posible inconsistencia entre el programa ejecutable y el package DB2.

Verificar:

* Versión desplegada.
* Package utilizado.
* Historial del despliegue.

Acción:

* Escalar a Desarrollo y DBA para investigar la consistencia de las versiones.

### SQLCODE -911 / -913

Posible deadlock o timeout.

Verificar:

* Mensaje y causa informada por DB2.
* Procesos concurrentes.
* Estado de la transacción.
* Si hubo rollback y qué parte del proceso llegó a completarse.

Acción:

* Confirmar la seguridad de la reejecución y seguir el procedimiento del equipo.

### SQLCODE -904

Recurso requerido no disponible.

Verificar:

* Recurso identificado en el mensaje.
* Estado del recurso y posibles restricciones.
* Si hay una incidencia de infraestructura o administración de DB2.

Acción:

* Escalar a DBA o al equipo responsable del recurso.

## 5. Archivos de entrada

### Archivo faltante o vacío

**Síntoma:** el proceso no encuentra el archivo esperado o recibe uno sin registros.

Verificar:

* Existencia y tamaño.
* Nombre y fecha operativa.
* Resultado de la transferencia.
* Cantidad de registros, si existe un control.
* Confirmación del equipo de origen.

Acción:

* Solicitar verificación o retransmisión al origen.
* No forzar condiciones ni reejecutar sin confirmar que el archivo es correcto y que el proceso puede repetirse de forma segura.

## 6. Micro Focus / Enterprise Server

### Región no disponible

Verificar:

* Estado de la región.
* Logs disponibles.
* Cambios recientes y dependencias.
* Alertas de infraestructura.

Acción:

* Seguir el procedimiento autorizado de recuperación y escalar si no se restablece.

### CICS transaction ABEND

Verificar:

* Código de ABEND.
* Transacción y programa.
* Mensajes y registros de diagnóstico disponibles.
* Datos involucrados, si corresponde.

Acción:

* Documentar evidencias y escalar al equipo responsable según la causa.

## Regla general

Un código orienta el diagnóstico, pero no demuestra por sí solo la causa raíz ni define automáticamente la solución.

Antes de realizar un rerun, cancelar un job, modificar una condición o corregir datos, verificar autorización, impacto y riesgo de duplicar operaciones.
