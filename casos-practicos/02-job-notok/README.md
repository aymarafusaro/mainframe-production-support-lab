# Caso 02 — Job terminado en NOTOK por dato numérico inválido

## 1. Descripción del incidente

**Fecha operativa:** 20261015  
**Job:** `ACTUALIZA-SALDOS`  
**Estado:** `ENDED NOTOK`  
**Paso fallido:** `ACTUALIZA`  
**Programa:** `ACTSALDO`  
**Código de retorno:** RC 0012  
**Job posterior:** `GENERA-REPORTE` — `WAIT CONDITION`

## 2. Impacto

La actualización de saldos no terminó. El job posterior quedó esperando la condición de éxito. Antes de recuperar el flujo, era necesario identificar el registro defectuoso y determinar el origen del dato inválido.

## 3. Investigación

### Evidencia 1 — Resultado del job

- `INICIALIZA`: RC 0000.
- `ACTUALIZA`: RC 0012.
- `CIERRE`: no ejecutado.
- `GENERA-REPORTE`: `WAIT CONDITION`.

### Evidencia 2 — Mensaje de error

El spool informó `INVALID NUMERIC DATA`, campo `WS-SALDO`, registro `00008427`.

La operación que falló fue:

```cobol
ADD IMPORTE-MOVIMIENTO TO WS-SALDO
```

### Evidencia 3 — Registro defectuoso

El archivo recibido contenía:

- Cuenta: `0015739201`
- Importe: `0000012500`
- Saldo recibido: `00008A4500`

El campo contenía una letra donde se esperaba un dígito.

### Evidencia 4 — Comparación con el origen

El sistema de origen confirmó el saldo `0000845000`. El diseño del archivo esperaba diez dígitos numéricos.

La comparación permitió descartar, en este escenario, una discrepancia en la definición esperada del campo.

### Evidencia 5 — Causa raíz

Los logs simulados indicaron que la generación de origen y la transferencia finalizaron correctamente, pero la interfaz no validó el contenido numérico antes de entregar el archivo.

**Causa raíz del escenario:** ausencia de validación del contenido del campo `SALDO` en la interfaz.

## 4. Recuperación segura

1. Escalar el incidente al equipo responsable de la interfaz.
2. Solicitar la regeneración del archivo desde el origen.
3. Validar el formato de los campos numéricos y los controles de integridad.
4. Obtener autorización para la recuperación.
5. Confirmar que la reejecución sea segura y no duplique actualizaciones.
6. Ejecutar según el procedimiento aprobado y monitorear los jobs dependientes.

No se corrigieron registros manualmente ni se forzaron condiciones.

## 5. Validación y cierre

La recuperación queda condicionada a recibir el archivo regenerado, validar sus controles y ejecutar el proceso con autorización.

Antes del cierre real, verificar:

- Resultado del job y RC de cada paso.
- Cantidad de registros procesados y rechazados.
- Conciliación de saldos.
- Estado de `GENERA-REPORTE`.
- Ausencia de duplicaciones por reejecución.

**Importante:** la investigación y la causa raíz están resueltas dentro de la simulación. No se inventan resultados de una reejecución que todavía no fue documentada.

## 6. Aprendizajes

- Investigar el paso que genera el RC no satisfactorio.
- Interpretar mensajes del spool y localizar el registro defectuoso.
- Comparar el archivo recibido con el origen y el copybook.
- Diferenciar una transferencia exitosa de una transferencia con contenido válido.
- Evitar cambios manuales sobre datos contables.
- Validar la seguridad de la reejecución y la conciliación antes del cierre.

> Caso educativo. Jobs, archivos, valores y logs ficticios.
