# Caso 01 — Job en WAIT CONDITION

## 1. Descripción del incidente

**Fecha operativa (ODATE):** 20261009
**Job bloqueado:** `LIQUIDA01`
**Estado inicial:** `WAIT CONDITION`
**Job predecesor:** `RECIBE01`
**Job de publicación:** `PUBLICA01`

El job `LIQUIDA01` no pudo continuar porque no se habían cumplido las condiciones de ejecución requeridas.

## 2. Impacto

La liquidación de pagos quedó demorada. La investigación debía determinar si el archivo de entrada había llegado, si correspondía a la fecha operativa y por qué no se había completado la cadena de procesamiento.

## 3. Investigación y evidencias

### Evidencia 1 — Alerta inicial

`LIQUIDA01` estaba en `WAIT CONDITION`. Se identificó como dependencia a `RECIBE01` y como archivo esperado `PAGOS.DIARIOS`.

**Acción:** revisar el job predecesor antes de reejecutar o modificar condiciones.

### Evidencia 2 — Error en la recepción

`RECIBE01` terminó `ENDED NOTOK`. El paso `VALIDAR-ARCHIVO` informó `DATASET NOT FOUND` y RC 0008.

**Acción:** revisar el JCL y confirmar el nombre del dataset buscado.

### Evidencia 3 — Archivo recibido

El archivo existía como `PROD.PAGOS.ENTRADA.20261008`. La validación simulada confirmó fecha operativa `20261009`, 12.500 registros y controles de recepción correctos.

**Conclusión parcial:** el archivo había llegado; el sufijo del dataset representaba su fecha de generación, no la fecha operativa.

### Evidencia 4 — Discrepancia de nombres

El JCL ejecutado buscaba `PROD.PAGOS.ENTRADA.DIARIO`, mientras que el dataset recibido tenía un nombre con sufijo de generación.

**Acción:** comparar el JCL ejecutado con la versión aprobada y revisar parámetros, procedimientos y jobs relacionados.

### Evidencia 5 — Dependencias del flujo

Se identificó que no se había pasado el parámetro de entrada esperado a `RECIBE01`. `PUBLICA01` dependía de la condición de éxito de `RECIBE01`, por lo que no se ejecutó. `LIQUIDA01` permaneció bloqueado.

## 4. Causa raíz

En esta simulación, la causa raíz fue una discrepancia en la configuración de ejecución: no se pasó el parámetro de entrada esperado y el job utilizó un nombre fijo de dataset que no estaba catalogado.

## 5. Recuperación

La recuperación fue autorizada por el equipo responsable.

1. Se corrigió la configuración conforme a la definición aprobada.
2. Se validó el dataset de entrada y su fecha operativa.
3. Se reejecutó `RECIBE01`.
4. Se verificó la publicación del archivo mediante `PUBLICA01`.
5. Se monitoreó `LIQUIDA01`.

No se forzaron condiciones ni se ejecutaron jobs manualmente sin autorización.

## 6. Validación final

Resultados ficticios de la simulación:

| Job         | Resultado |   RC |
| ----------- | --------- | ---: |
| `RECIBE01`  | ENDED OK  | 0000 |
| `PUBLICA01` | ENDED OK  | 0000 |
| `LIQUIDA01` | ENDED OK  | 0000 |

Conciliación final:

* Registros leídos: 12.500
* Registros procesados: 12.500
* Registros rechazados: 0
* Diferencia: 0

## 7. Cierre del incidente

Se documentaron el diagnóstico, las acciones correctivas, las ejecuciones y la conciliación. En el escenario simulado, el incidente pudo cerrarse después de verificar la recuperación completa.

## 8. Aprendizajes

* Interpretar `WAIT CONDITION` y `ENDED NOTOK`.
* Revisar el spool y el JCL ejecutado.
* Diferenciar DDNAME, DSN y ODATE.
* Verificar dependencias entre jobs.
* No asumir que un archivo inexistente bajo un nombre implica que no fue recibido.
* Evitar reejecuciones y cambios sin validar el impacto y obtener autorización.
* Confirmar los resultados técnicos y funcionales antes del cierre.

> **Nota:** este caso es educativo. Los jobs, registros y resultados son ficticios.
