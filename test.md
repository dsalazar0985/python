```mermaid
flowchart TD
    A[Inicio del Proyecto] --> B[Identificación del Cliente o Usuario]
    B --> C[Definición de Objetivos]
    C --> D[Recolección de Información con stakeholders]

    D --> D1[Entrevistas y Encuestas]
    D --> D2[Revisión de Documentación Existente]
    D --> D3[Observación Directa]

    D1 --> E[Análisis de Requisitos]
    D2 --> E
    D3 --> E

    E --> E1[Identificación de Requisitos Funcionales y No Funcionales]
    E --> E2[Priorización de Requisitos]

    E1 --> F[Documentación de Requisitos -  Jira]
    E2 --> F

    F --> F1[Creación de Especificaciones]
    F --> F2[Validación y Aprobación de Requisitos]

    F2 --> G[Gestión de Cambios en los Requisitos]

    G --> G1[Establecimiento de un Proceso de Control de Cambios]
    G --> G2[Comunicación de Cambios]
    G1 --> G3["` -Evaluacion
    -Analisis de impacto
    -Revision y aprobacion`" ]

    G2 --> H[Finalización del Proceso]

    H --> H1[Revisión Final]
    H --> H2[Aprobación y Cierre]
