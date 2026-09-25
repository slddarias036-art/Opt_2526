# Eight Academy – Sistema de inscripción de optativas

## Descripción general

Este repositorio contiene la interfaz web para un sistema de inscripción de materias optativas de la institución educativa Eight Academy. El sistema está diseñado para permitir que un estudiante:

- busque su nombre en la lista oficial,
- seleccione el nivel correspondiente (Básica o Bachillerato),
- elija una materia optativa disponible,
- confirme la inscripción y reciba una confirmación visual.

También incluye un panel administrativo para consultar los cupos y ver el listado de estudiantes inscritos, así como exportar la información en formato CSV.

## ¿Qué hace el sistema?

### 1) Módulo de inscripción para estudiantes
El archivo principal `index.html` presenta una experiencia de registro digital con estas etapas:

1. Búsqueda del estudiante por nombre.
2. Validación del nivel, curso y paralelo en base a la lista oficial.
3. Visualización de las materias optativas según el nivel del alumno.
4. Control de cupos por materia.
5. Confirmación final de la inscripción.

La interfaz incluye:

- autocompletado de nombres,
- sugerencias de coincidencias,
- selección por radio buttons,
- mensajes de error o éxito,
- actualización automática de cupos desde la API.

### 2) Módulo administrativo
El archivo `admin.html` ofrece un panel para revisar el estado del proceso:

- cupos por materia,
- conteos por nivel,
- listado completo de inscritos,
- exportación del registro a CSV.

Esto permite que el personal administrativo supervise la inscripción sin necesidad de revisar manualmente cada solicitud.

## Arquitectura del sistema

El proyecto está compuesto por dos páginas estáticas en HTML + CSS + JavaScript:

- `index.html`: página pública para estudiantes.
- `admin.html`: panel administrativo.

La lógica de negocio y persistencia no está implementada directamente aquí. En su lugar, ambos archivos se conectan a un endpoint de Google Apps Script definido en la constante:

```javascript
const API_URL = "https://script.google.com/macros/s/.../exec";
```

Ese endpoint es el responsable de:

- consultar la lista de cupos,
- registrar la inscripción del estudiante,
- devolver la información actualizada al frontend,
- servir datos al panel administrativo.

En otras palabras, este repositorio funciona como una capa frontal del sistema, mientras la lógica del backend queda en Google Apps Script.

## Cómo trabaja el flujo

### Flujo del estudiante

1. El estudiante escribe su nombre en el buscador.
2. El sistema filtra la lista oficial de estudiantes y muestra coincidencias.
3. Al seleccionar un estudiante, se completan automáticamente sus datos:
   - nivel,
   - curso,
   - paralelo,
   - nombre completo.
4. Se muestran las materias disponibles de acuerdo con el nivel seleccionado.
5. El usuario elige una materia con cupo disponible.
6. El frontend envía una solicitud POST a la API con los siguientes datos:
   - nombre,
   - curso,
   - paralelo,
   - nivel,
   - materia,
   - origen.
7. La API valida y registra la inscripción.
8. El sistema actualiza los cupos y muestra un mensaje final de éxito.

### Flujo administrativo

1. El administrador abre `admin.html`.
2. Ingresa la URL del Google Apps Script.
3. El panel consulta `?admin=1` para recuperar los datos.
4. El sistema muestra:
   - resumen por nivel,
   - conteo de cupos por materia,
   - lista de todos los inscritos.
5. El administrador puede exportar la información a CSV.

## Estructura del repositorio

```text
Opt_2526/
├── index.html        # Interfaz para que los estudiantes inscriban optativas
├── admin.html        # Panel administrativo para revisar cupos e inscritos
└── README.md         # Documentación del proyecto
```

## Datos principales manejados

El sistema trabaja con información como:

- nombre del estudiante,
- nivel (Básica o Bachillerato),
- curso,
- paralelo,
- materia seleccionada,
- fecha y hora de inscripción,
- cupos disponibles por materia.

## Configuración requerida

Para que el sistema funcione completo, se debe configurar la URL del backend de Google Apps Script en los archivos:

- `index.html` en la variable `API_URL`
- `admin.html` al conectarse desde el panel administrativo

Ejemplo:

```javascript
const API_URL = "https://script.google.com/macros/s/AKfycbx4DxNzsHBOvOs5N8l-FQArV35e_EJ25aSMopSKbtL4md-sXSszCc1-aDGrw4XKGStTBw/exec";
```

## Requisitos

- navegador web moderno,
- acceso a Internet,
- URL válida de Google Apps Script configurada,
- lista oficial de estudiantes (pre-cargada en `index.html` o gestionada en el backend).

## Observaciones del proyecto

- El código es estático y frontal; no hay un servidor Node.js, Python ni base de datos en este repositorio.
- La lógica de negocio se externaliza a Google Apps Script, lo que permite manejar inscripciones y cupos de forma centralizada.
- La UI está construida con HTML, CSS y JavaScript puro, sin frameworks.

## Resumen breve

Este sistema automatiza la inscripción de optativas para estudiantes de Eight Academy, controlando la selección, validación y cupos de cada materia, además de ofrecer una vista administrativa para supervisar el proceso y exportar la información.

## Autor / contexto

Proyecto orientado a gestión escolar, con enfoque en inscripción de materias optativas para diferentes niveles académicos.
