# Caso 05 — DB2 SQLCODE -911 por deadlock

## 1. Resumen del incidente

- ODATE: 20261017
- Job: ACTUALIZA-CUENTAS
- Step: ACTUALIZAR
- Programa víctima: ACTCTA01
- SQLCODE: -911
- SQLSTATE: 40001
- Reason code informado: 00C90088
- Job dependiente: GENERA-RESUMEN

Caso simulado con datos ficticios.

## 2. Síntoma

El job ACTUALIZA-CUENTAS terminó con error durante una operación UPDATE sobre CUENTAS. El job GENERA-RESUMEN quedó esperando.

## 3. Evidencia

El análisis simulado del deadlock indica:

- ACTCTA01 mantiene bloqueada la fila 00078421 y espera la 00091234.
- ACTCTA02 mantiene bloqueada la fila 00091234 y espera la 00078421.
- DB2 seleccionó la transacción A como víctima.
- La unidad de trabajo afectada fue revertida.

## 4. Diagnóstico

La evidencia es consistente con un deadlock: ambas transacciones esperan recursos retenidos por la otra.

Causa probable: los programas acceden a las filas en distinto orden. Debe confirmarse revisando la lógica transaccional y las sentencias SQL de ambos programas.

SQLCODE -911 por sí solo no demuestra que todo el job haya sido revertido. Se debe determinar el alcance de la unidad de trabajo y revisar las operaciones previamente confirmadas.

## 5. Plan de resolución

1. Revisar los mensajes de DB2 y el análisis del deadlock.
2. Identificar los recursos, las transacciones y la unidad de trabajo víctima.
3. Revisar el orden de acceso a filas en ACTCTA01 y ACTCTA02.
4. Coordinar con desarrollo una estrategia de acceso consistente, si se confirma que el orden inverso causa el deadlock.
5. Verificar el alcance del rollback y las operaciones que ya pudieron quedar confirmadas.
6. Obtener autorización operativa y evaluar el reintento de la unidad de trabajo afectada.
7. Evitar duplicaciones antes de cualquier reejecución.
8. Verificar el SQLCODE, los registros actualizados y la consistencia de los saldos.
9. Permitir que GENERA-RESUMEN continúe únicamente cuando se satisfagan sus dependencias y controles.

## 6. Prevención

- Revisar el orden de acceso a recursos compartidos.
- Analizar la duración y el alcance de las unidades de trabajo.
- Implementar reintentos controlados únicamente donde estén previstos y autorizados.
- Registrar SQLCODE, SQLSTATE y mensajes relevantes para facilitar el diagnóstico.

## 7. Estado final

Diagnóstico probable documentado.
Plan de recuperación propuesto; ejecución no confirmada.
No se afirma un RC exitoso.

## 8. Alcance

Simulación de laboratorio con datos ficticios. No representa una intervención en producción.
