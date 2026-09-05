# Diagrama Entidad-Relación
```mermaid
erDiagram
    PERSONA {
        string nombre
        string apellido
        string correo
        string telefono
    }
    ESTUDIANTE {
        string codigo
        string carrera
        int semestre
    }
    TUTOR {
        int id_tutor
        string tipo
    }
    MATERIA {
        string id_materia
        string nombre
    }
    TEMA {
        string id_tem
        string id_materia
        string nombre_tema
    }
    ESPACIO {
        string id_espacio
        string tipo
        string nombre_ubicacion
    }
    SESION_TUTORIA {
        string id_sesion
        string id_solicitud
        string id_espacio
        string fecha
        string hora_inicio
        string hora_fin
        string estado
    }
    MATRICULA_PERIODO {
        string id_matricula
        string codigo
        string periodo_academico
        string promedio_periodo
        string estado_matricula
    }
    HISTORIAL_ACADEMICO {
        string id_historial
        string codigo
        string id_materia
        string periodo_academico
        string calificacion_final
    }
    CALIFICACION_PARCIAL {
        string id_calificacion
        string codigo
        string id_materia
        string nota
    }
    ALERTA_TEMPRANA {
        string id_alerta
        string codigo
        string id_materia
        string nivel_riesgo
    }
    SOLICITUD_TUTORIA {
        string id_solicitud
        string codigo
        string id_materia
        int id_tutor_asignado
        string id_alerta_origen
        string estado
    }
    ESPECIALIDAD_TUTOR {
        string id_especialidad
        int id_tutor
        string id_tema
        string id_historial_validacion
    }
    DISPONIBILIDAD_TUTOR {
        string id_disponibilidad
        int id_tutor
        string dia_semana
        string hora_inicio
        string hora_fin
    }
    ASISTENCIA {
        string id_asistencia
        string id_sesion
        string codigo
        string asistio
    }
    RETROALIMENTACION {
        string id_retro
        string id_sesion
        string codigo
        int id_tutor
        string calificacion
    }

    PERSONA ||--o| ESTUDIANTE : es
    PERSONA ||--o| TUTOR : es
    ESTUDIANTE ||--|{ MATRICULA_PERIODO : tiene
    ESTUDIANTE ||--|{ HISTORIAL_ACADEMICO : tiene
    ESTUDIANTE ||--|{ CALIFICACION_PARCIAL : obtiene
    ESTUDIANTE ||--o{ ALERTA_TEMPRANA : recibe

    MATERIA ||--|{ TEMA : contiene

    TUTOR ||--|{ ESPECIALIDAD_TUTOR : certifica
    TUTOR ||--|{ DISPONIBILIDAD_TUTOR : define

    ESTUDIANTE |o--|{ SOLICITUD_TUTORIA : crea
    MATERIA ||--o{ SOLICITUD_TUTORIA : corresponde
    ALERTA_TEMPRANA |o--o| SOLICITUD_TUTORIA : origina
    TUTOR |o--o| SOLICITUD_TUTORIA : asigna

    SOLICITUD_TUTORIA ||--o| SESION_TUTORIA : agenda
    ESPACIO ||--o{ SESION_TUTORIA : aloja

    SESION_TUTORIA ||--|{ ASISTENCIA : asiste
    SESION_TUTORIA ||--o| RETROALIMENTACION : evalua
    ESTUDIANTE ||--o{ ASISTENCIA : registra
    ESTUDIANTE |o--o{ RETROALIMENTACION : evalua
    TUTOR |o--o{ RETROALIMENTACION : evaluado

    HISTORIAL_ACADEMICO |o--o| ESPECIALIDAD_TUTOR : valida
    TEMA |o--o{ ESPECIALIDAD_TUTOR : cubre
