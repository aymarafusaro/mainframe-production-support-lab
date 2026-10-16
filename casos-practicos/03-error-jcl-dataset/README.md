# Caso 03 — Error de JCL por nombre de dataset incorrecto

## 1. Descripción del incidente

**ODATE:** 20261016  
**Job:** `EXTRAE-MOVIMIENTOS`  
**Estado:** `JCL ERROR`  
**DDNAME:** `ENTRADA`  
**Mensaje:** `DATA SET NOT FOUND`

El job intentó acceder a `PROD.MOVIMIENTOS.DIARIO`, nombre que no estaba catalogado.

## 2. Impacto

La extracción de movimientos no pudo comenzar. El job posterior `ENVIA-REPORTES` quedó pendiente de la finalización correcta del proceso.

## 3. Investigación

### Evidencia 1 — Catálogo

La consulta simulada mostró:

- `PROD.MOVIMIENTOS.DIARIO`: no catalogado.
- `PROD.MOVIMIENTOS.DIARIO.20261016`: catalogado.
- `PROD.MOVIMIENTOS.DIARIO.20261015`: catalogado.

La existencia de un dataset no demuestra por sí sola que corresponda a la fecha operativa.

### Evidencia 2 — Diferencia de JCL

JCL ejecutado:

```jcl
//ENTRADA DD DSN=PROD.MOVIMIENTOS.DIARIO,
//           DISP=SHR
```

JCL aprobado en la simulación:

```jcl
//         SET FECHA=20261016
//ENTRADA  DD DSN=PROD.MOVIMIENTOS.DIARIO.&FECHA,
//            DISP=SHR
```

El JCL ejecutado utilizó un nombre fijo y omitió el parámetro de fecha definido en la versión aprobada.

### Evidencia 3 — Validaciones necesarias

Antes de recuperar el proceso se debe:

1. Confirmar que la ODATE sea `20261016`.
2. Verificar que el dataset correspondiente exista y sea el correcto.
3. Comparar la configuración desplegada con la versión aprobada.
4. Solicitar autorización para corregir la configuración.
5. Verificar el resultado del job y el estado de sus dependencias.

## 4. Causa raíz

En este escenario simulado, el JCL ejecutado no coincidía con el aprobado: utilizaba un nombre fijo de dataset en lugar de construir el nombre con el parámetro `FECHA`.

## 5. Recuperación segura

La recuperación se realiza después de la autorización del equipo responsable:

1. Corregir el JCL o la configuración conforme a la versión aprobada.
2. Validar la ODATE y el nombre del dataset resultante.
3. Confirmar que el archivo de entrada sea el correcto.
4. Ejecutar el job según el procedimiento autorizado.
5. Revisar el spool, el código de retorno y el resultado funcional.
6. Confirmar el estado de `ENVIA-REPORTES`.

No se debe utilizar el archivo del día anterior ni cambiar `DISP` como solución improvisada.

## 6. Resultado y cierre

La causa del error quedó identificada en la simulación. La recuperación debe considerarse completa únicamente después de confirmar la ejecución satisfactoria, la integridad de la extracción y el estado del job posterior.

No se asigna un RC exitoso ficticio a una ejecución que no fue observada.

## 7. Aprendizajes

- Interpretar `JCL ERROR` y `DATA SET NOT FOUND`.
- Diferenciar DDNAME y DSN.
- Consultar el catálogo antes de modificar el JCL.
- Comprender el uso de parámetros simbólicos como `&FECHA`.
- Comparar el JCL ejecutado con la versión aprobada.
- Validar la fecha operativa, la entrada y las dependencias antes de cerrar.

> Caso educativo. Jobs, nombres y resultados ficticios.
