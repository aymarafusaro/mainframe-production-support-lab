# Laboratorio de Soporte de Producción Mainframe

Repositorio práctico para aprender diagnóstico y resolución de incidencias en entornos Mainframe.

## Objetivos

- Interpretar estados de jobs y códigos de retorno.
- Diagnosticar errores de JCL, datasets y VSAM.
- Investigar abends de programas COBOL.
- Comprender incidencias de DB2.
- Verificar problemas de recepción y procesamiento de archivos.
- Aprender cuándo corresponde reintentar una ejecución y cuándo escalar.
- Documentar incidencias mediante tickets y reportes de guardia.

## Tecnologías y herramientas

- COBOL
- JCL
- IBM z/OS
- JES y spool de jobs
- Control-M
- VSAM y datasets
- DB2 y SQL
- Jira
- Micro Focus / Enterprise Server (conceptos y simulaciones)

## Estructura del repositorio

* [`docs/`](docs/): checklist de incidentes, catálogo de errores y procedimiento de guardia.
* [`casos-practicos/`](casos-practicos/): seis casos documentados con evidencias y planes de recuperación.

  * [Caso 01 — Job WAIT CONDITION](casos-practicos/01-job-wait-condition/README.md)
  * [Caso 02 — Job NOTOK](casos-practicos/02-job-notok/README.md)
  * [Caso 03 — Error JCL y dataset](casos-practicos/03-error-jcl-dataset/README.md)
  * [Caso 04 — ABEND COBOL S0C7](casos-practicos/04-abend-cobol-s0c7/README.md)
  * [Caso 05 — Deadlock DB2](casos-practicos/05-error-db2/README.md)
  * [Caso 06 — Archivo no recibido](casos-practicos/06-archivo-no-recibido/README.md)
* [`simulaciones/logs/`](simulaciones/logs/): registros ficticios para practicar el análisis de ejecuciones.
* [`simulaciones/jcl/`](simulaciones/jcl/): ejemplos didácticos de JCL.
* [`simulaciones/archivos/`](simulaciones/archivos/): archivos de prueba con datos ficticios.
* [`plantillas/`](plantillas/): modelos de tickets Jira y reportes de guardia.


## Metodología

Cada caso práctico incluye:

1. Descripción de la incidencia.
2. Impacto potencial.
3. Evidencias disponibles.
4. Preguntas de diagnóstico.
5. Análisis de causa probable.
6. Acciones seguras y criterios de escalamiento.
7. Documentación del incidente.

## Importante

Los escenarios y registros de este repositorio son didácticos.
No se deben ejecutar acciones de producción sin autorización,
validación del impacto y aplicación de los procedimientos del equipo.

