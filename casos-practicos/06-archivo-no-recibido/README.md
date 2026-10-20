# Caso 06 — Archivo de entrada no recibido

## 1. Resumen del incidente

- ODATE: 20261020
- Proceso de recepción: RECEPCION-PAGOS
- Job afectado: PROCESA-PAGOS
- Job dependiente: LIQUIDA-PAGOS
- Condición Control-M: ARCHIVO-PAGOS-RECIBIDO
- Dataset esperado: PROD.PAGOS.ENTRADA.20261020

Caso simulado con datos ficticios.

## 2. Síntoma

PROCESA-PAGOS permanece esperando porque el dataset esperado no está disponible bajo el nombre definitivo. La condición ARCHIVO-PAGOS-RECIBIDO no está establecida y LIQUIDA-PAGOS no inició.

## 3. Evidencia

- La transferencia del archivo terminó correctamente.
- La validación inicial del archivo terminó correctamente.
- El paso CATALOGAR no se ejecutó.
- El dataset esperado no aparece en el catálogo.
- El archivo recibido se identifica como PAGOS_DIARIO_20261020.DAT.

## 4. Diagnóstico

La causa inmediata en la simulación es que el proceso de recepción no completó el paso de catalogación del dataset definitivo.

Queda pendiente determinar por qué no se ejecutó ese paso y confirmar la ubicación y la integridad del archivo antes de recuperarlo.

## 5. Plan de resolución

1. Revisar el log completo de RECEPCION-PAGOS y determinar por qué no se ejecutó CATALOGAR.
2. Confirmar la ubicación temporal del archivo.
3. Validar la fecha operativa, la estructura, la cantidad de registros y la integridad del archivo.
4. Coordinar con el equipo responsable la corrección del paso fallido.
5. Confirmar que PROD.PAGOS.ENTRADA.20261020 exista y contenga el archivo correcto.
6. Verificar la condición ARCHIVO-PAGOS-RECIBIDO en Control-M.
7. Liberar la condición únicamente mediante el procedimiento autorizado y después de confirmar la recepción completa.
8. Ejecutar PROCESA-PAGOS según el procedimiento operativo.
9. Revisar el resultado y confirmar que LIQUIDA-PAGOS pueda continuar sin duplicaciones.

## 6. Prevención

- Controlar el resultado de cada etapa de recepción.
- Generar alertas cuando falle la catalogación.
- Verificar la existencia y la integridad del dataset antes de establecer la condición.
- Mantener trazabilidad entre transferencia, validación, catalogación y procesamiento.

## 7. Estado final

Diagnóstico de la falla inmediata documentado.
Plan de recuperación propuesto; ejecución no confirmada.
No se afirma que el archivo haya sido catalogado ni que los jobs hayan finalizado correctamente.

## 8. Alcance

Caso práctico de laboratorio con datos ficticios. No representa una intervención en producción.
