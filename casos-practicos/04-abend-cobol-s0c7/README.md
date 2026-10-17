# Caso 04 — ABEND S0C7 en cálculo de intereses

## 1. Resumen del incidente

- Fecha operativa (ODATE): 20261017
- Job: CALCULA-INTERESES
- Step: CALCULAR
- Programa: CALINT01
- Estado: ABEND S0C7
- Registro informado: 00003142
- Job dependiente: GENERA-RESUMEN
- Impacto: el resumen permanece pendiente hasta resolver el incidente.

## 2. Síntoma

El job termina con ABEND S0C7 durante la operación:

```cobol
COMPUTE WS-INTERES =
    WS-SALDO * WS-TASA / 100
```

El ABEND S0C7 suele estar relacionado con datos decimales inválidos utilizados en una operación numérica.

## 3. Evidencia analizada

Registro informado:

- Cuenta: 00078421

- Saldo recibido: 000015A250

- Tasa recibida: 0000350

Definiciones COBOL:

```cobol
01 WS-SALDO   PIC 9(8)V99 COMP-3.
01 WS-TASA    PIC 9(3)V99 COMP-3.
01 WS-INTERES PIC 9(9)V99 COMP-3.
```

El campo SALDO contiene una letra A donde se esperaba un dígito, según el formato informado.

El flujo analizado mueve los campos y ejecuta el cálculo sin una validación numérica explícita previa.

## 4. Diagnóstico

Causa probable: dato de entrada inválido y validación insuficiente antes de la operación numérica.

La causa debe confirmarse contrastando el archivo real, las posiciones del layout, el copybook y la lógica de lectura y conversión.

COMP-3 utiliza representación decimal empaquetada. La V indica una posición decimal implícita; no representa una coma ni un punto almacenado en el campo.

## 5. Plan de recuperación

1. Solicitar al equipo de origen la regeneración del archivo desde una fuente confiable.

2. Validar estructura, posiciones, campos numéricos, cantidad de registros e integridad.

3. Revisar si hubo actualizaciones parciales antes del ABEND.

4. Obtener autorización y seguir el procedimiento de recuperación aprobado.

5. Verificar el RC, los registros procesados, los saldos y la conciliación.

6. Liberar GENERA-RESUMEN únicamente cuando las validaciones requeridas sean satisfactorias.

7. No modificar manualmente el registro ni forzar condiciones de ejecución sin autorización.

## 6. Prevención

- Validar campos numéricos antes de utilizarlos en cálculos.

- Rechazar o separar registros inválidos según las reglas del sistema.

- Registrar el número de registro y el motivo del rechazo.

- Incorporar pruebas para datos alfanuméricos en campos numéricos.

- Definir con el equipo funcional el tratamiento de registros rechazados.

## 7. Estado final

Diagnóstico probable documentado y plan de recuperación propuesto. La recuperación efectiva no está confirmada en esta simulación.

## 8. Alcance

Caso práctico de laboratorio con datos ficticios. No representa una intervención en producción. 

