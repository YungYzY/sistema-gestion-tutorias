erDiagram
    %% --- ENTIDADES PRINCIPALES ---
    PERSONA {
        string nombre
        string apellido
        string correo
        string telefono
    }
    ESTUDIANTE {
        string codigo PK
        string carrera
        int semestre
    }
    TUTOR {
        int id_tutor PK
        string tipo
    }
    MATERIA {
        string id_materia PK
        string nombre
    }
    TEMA {
        string id_tema PK
        string id_materia FK
        string nombre_tema
    }
    ESPACIO {
        string id_espacio PK
        string tipo
        string nombre_ubicacion
    }
    SESION_TUTORIA {
        string id_sesion PK
        string id_solicitud FK
        string id_espacio FK
        date fecha
        time hora_inicio
        time hora_fin
        string estado
    }

    %% --- ENTIDADES DE RELACIÓN/TRANSACCIONALES ---
    MATRICULA_PERIODO {
        string id_matricula PK
        string codigo FK
        string periodo_academico
        float promedio_periodo
        string estado_matricula
    }
    HISTORIAL_ACADEMICO {
        string id_historial PK
        string codigo FK
        string id_materia FK
        string periodo_academico
        float calificacion_final
    }
    CALIFICACION_PARCIAL {
        string id_calificacion PK
        string codigo FK
        string id_materia FK
        float nota
    }
    ALERTA_TEMPRANA {
        string id_alerta PK
        string codigo FK
        string id_materia FK
        string nivel_riesgo
    }
    SOLICITUD_TUTORIA {
        string id_solicitud PK
        string codigo FK
        string id_materia FK
        int id_tutor_asignado FK "nullable"
        string id_alerta_origen FK "nullable"
        string estado
    }
    ESPECIALIDAD_TUTOR {
        string id_especialidad PK
        int id_tutor FK
        string id_tema FK
        string id_historial_validacion FK
    }
    DISPONIBILIDAD_TUTOR {
        string id_disponibilidad PK
        int id_tutor FK
        string dia_semana
        time hora_inicio
        time hora_fin
    }
    ASISTENCIA {
        string id_asistencia PK
        string id_sesion FK
        string codigo FK
        boolean asistio
    }
    RETROALIMENTACION {
        string id_retro PK
        string id_sesion FK
        string codigo "FK, evaluador"
        int id_tutor "FK, evaluado"
        int calificacion
    }

    %% --- HERENCIAS (Implementación en Mermaid) ---
    %% Mermaid no soporta herencia explícita, se usa relación 1:1
    PERSONA ||--o| ESTUDIANTE : es
    PERSONA ||--o| TUTOR : es

    %% --- RELACIONES (Basadas en el DER y FKs) ---
    
    %% Estudiante - Académico
    ESTUDIANTE ||--|{ MATRICULA_PERIODO : tiene
    ESTUDIANTE ||--|{ HISTORIAL_ACADEMICO : tiene
    ESTUDIANTE ||--|{ CALIFICACION_PARCIAL : obtiene
    ESTUDIANTE ||--o{ ALERTA_TEMPRANA : recibe
    
    %% Materia - Temas
    MATERIA ||--|{ TEMA : contiene
    
    %% Tutor - Capacidades y Disponibilidad
    TUTOR ||--|{ ESPECIALIDAD_TUTOR : certifica
    TUTOR ||--|{ DISPONIBILIDAD_TUTOR : define
    
    %% Solicitud Tutoría (Origen, Asignación, Materia/Tema)
    ESTUDIANTE |o--|{ SOLICITUD_TUTORIA : crea
    MATERIA ||--o{ SOLICITUD_TUTORIA : corresponde
    ALERTA_TEMPRANA |o--o| SOLICITUD_TUTORIA : origina
    TUTOR |o--o| SOLICITUD_TUTORIA : asigna
    
    %% Sesión Tutoría (Agendamiento, Espacio)
    SOLICITUD_TUTORIA ||--o| SESION_TUTORIA : agenda
    ESPACIO ||--o{ SESION_TUTORIA : aloja
    
    %% Asistencia y Retroalimentación (Vinculadas a Sesión)
    SESION_TUTORIA ||--|{ ASISTENCIA : asiste
    SESION_TUTORIA ||--o| RETROALIMENTACION : evalua
    ESTUDIANTE ||--o{ ASISTENCIA : registra
    ESTUDIANTE |o--o{ RETROALIMENTACION : evalua
    TUTOR |o--o{ RETROALIMENTACION : evaluado
    
    %% Especialidad Validada por Historial (Relación ternaria simulada)
    HISTORIAL_ACADEMICO |o--o| ESPECIALIDAD_TUTOR : valida
    TEMA |o--o{ ESPECIALIDAD_TUTOR : cubre
