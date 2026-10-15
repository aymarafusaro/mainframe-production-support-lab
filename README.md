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

- `docs/`: procedimientos, checklist y catálogo de errores.
- `casos-practicos/`: ejercicios de diagnóstico.
- `simulaciones/logs/`: registros de ejecución ficticios para practicar.
- `simulaciones/jcl/`: ejemplos de JCL.
- `simulaciones/archivos/`: archivos de prueba.
- `plantillas/`: modelos de tickets y reportes de guardia.

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

