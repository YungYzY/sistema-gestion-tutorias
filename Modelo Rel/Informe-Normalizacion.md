# Informe de Normalización de Base de Datos

## 1. Primera Forma Normal (1FN)
* **Objetivo:** Garantizar la atomicidad de los atributos y eliminar grupos repetitivos.
* **Aplicación en el modelo:** Los atributos de la entidad `Persona`, como `Nombre`, `Apellido`, `Correo` y `Telefono`, se estructuran para contener valores únicos y atómicos por registro. Para soportar múltiples datos de contacto sin violar esta regla, estos se asumen como tablas independientes.

## 2. Segunda Forma Normal (2FN)
* **Objetivo:** Asegurar que los atributos no clave dependan funcionalmente por completo de la clave principal.
* **Aplicación en el modelo:** El modelo cumple esta forma normal al utilizar identificadores únicos. En casos de claves lógicas compuestas, como en el `Historial_Academico` atributos como la `calificacion_final` dependen de la combinación total del estudiante, la materia y el periodo académico, garantizando la dependencia completa.

## 3. Tercera Forma Normal (3FN)
* **Objetivo:** Eliminar dependencias transitivas, asegurando que ningún atributo no clave dependa de otro atributo no clave.
* **Aplicación en el modelo:** La entidad `Estudiante` incluye directamente su `carrera`, sin arrastrar datos adicionales que generen dependencias externas. Asimismo, la `Sesion_Tutoria` se asocia a un `Tema` específico, lo que permite inferir la `Materia` a través de sus relaciones establecidas, eliminando cualquier redundancia estructural.
