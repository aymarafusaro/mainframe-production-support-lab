# Resolución — Caso 04: ABEND S0C7

## Estado

Diagnóstico preliminar documentado. Resolución propuesta; ejecución no confirmada.

## Causa probable

El registro 00003142 contiene el valor `000015A250` para SALDO. El carácter `A` no corresponde a un campo compuesto exclusivamente por dígitos y puede provocar una excepción decimal cuando el dato se utiliza en un cálculo numérico.

El fragmento revisado mueve el campo de entrada a `WS-SALDO` y realiza el cálculo sin mostrar una validación numérica previa.

Antes de declarar la causa raíz definitiva, se deben verificar el layout del archivo, el copybook, las posiciones de los campos y las instrucciones de lectura y conversión.

## Plan de resolución

1. Revisar el spool y el dump para confirmar la instrucción y el contexto del ABEND.
2. Comparar el registro 00003142 con el layout y el copybook vigentes.
3. Confirmar con el equipo de origen el valor correcto de SALDO y solicitar la regeneración desde una fuente confiable.
4. Validar el archivo corregido: estructura, campos numéricos, cantidad de registros e integridad.
5. Revisar si el job realizó actualizaciones parciales antes del ABEND y determinar cómo evitar duplicaciones.
6. Corregir o reforzar la validación del programa mediante el cambio aprobado y las pruebas correspondientes.
7. Obtener autorización operativa y ejecutar el procedimiento de recuperación definido por el equipo responsable.
8. Revisar el RC final, los registros procesados, los saldos y la conciliación.
9. Permitir que GENERA-RESUMEN continúe únicamente después de confirmar que se cumplen sus dependencias y controles.

## Validaciones técnicas pendientes

- Confirmar la correspondencia entre las posiciones del registro y los campos COBOL.
- Investigar la diferencia entre el campo TASA de 7 posiciones informado en el layout y WS-TASA definido como PIC 9(3)V99.
- Definir el tratamiento esperado para registros inválidos: rechazo, cuarentena o proceso de excepción.
- Verificar que los datos se validen antes de moverlos a campos numéricos o utilizarlos en cálculos.

## Criterios de cierre

El incidente podrá considerarse resuelto cuando el archivo sea válido, la ejecución autorizada finalice correctamente, no existan actualizaciones duplicadas y los resultados estén conciliados.

No se registra un RC exitoso ni una recuperación efectiva porque no hay evidencia de que se haya ejecutado.

## Prevención

- Validar los campos numéricos de entrada.
- Registrar número de registro y motivo del rechazo.
- Incorporar casos de prueba con letras en campos numéricos.
- Mantener sincronizados los layouts, copybooks y programas.
- Documentar el procedimiento de recuperación y sus autorizaciones.

## Alcance

Caso práctico de laboratorio con datos ficticios. No representa una intervención en producción.
