# 🎓 Eight Academy – Sistema de Inscripción de Materias Optativas

## Descripción

Sistema web integrado para la gestión de inscripciones de materias optativas en Eight Academy, período lectivo 2026-2027. Proporciona una solución completa que incluye interfaz de inscripción para estudiantes y panel administrativo para la supervisión de cupos e inscritos.

**Versión:** 2.0  
**Enfoque:** Estudiantes de Educación Básica y Bachillerato  
**Tecnología:** HTML5 + CSS3 + JavaScript (Frontend Estático)

---

## Características principales

### 📝 Módulo de inscripción para estudiantes

- **Búsqueda inteligente:** autocompletado de nombres desde la lista oficial institucional
- **Validación automática:** verificación de datos del estudiante (nivel, curso, paralelo)
- **Selección de optativas:** interfaz visual con control de cupos en tiempo real
- **Sincronización de cupos:** actualización automática cada 15 segundos
- **Confirmación inmediata:** recepción de confirmación visual al completar la inscripción
- **Diseño responsivo:** experiencia optimizada para dispositivos móviles y de escritorio

### 📊 Panel administrativo

- **Resumen de cupos:** visualización por materia y nivel educativo
- **Listado de inscritos:** tabla completa con datos (fecha, nombre, nivel, curso, paralelo, materia)
- **Exportación de datos:** descarga en formato CSV para análisis posterior
- **Actualización automática:** refresco de datos cada 20 segundos
- **Acceso seguro:** integración mediante URL de Google Apps Script

---

## Arquitectura del sistema

### Componentes

```
┌─────────────────────────────────────────────────────────────┐
│                      FRONTEND (Este Repo)                   │
├─────────────────────────────────────────────────────────────┤
│  index.html        → Interfaz pública para estudiantes      │
│  admin.html        → Panel administrativo                   │
│  README.md         → Documentación técnica                  │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ↓ (API REST)
┌─────────────────────────────────────────────────────────────┐
│          BACKEND (Google Apps Script)                       │
├─────────────────────────────────────────────────────────────┤
│  • Gestión de cupos                                         │
│  • Validación de inscripciones                              │
│  • Persistencia en Google Sheets                            │
│  • Endpoint API (GET/POST)                                  │
└─────────────────────────────────────────────────────────────┘
```

### Flujo de datos

1. **Estudiante accede** a `index.html`
2. **Busca su nombre** → consulta lista offline (precargada)
3. **Selecciona materia** → el sistema valida cupos (solicitud a API)
4. **Envía inscripción** → POST a Google Apps Script
5. **Recibe confirmación** → actualización de cupos en tiempo real

---

## Instrucciones de uso

### Acceso para estudiantes

```
https://github.com/slddarias036-art/Opt_2526
```

Abrir el archivo `index.html` desde el navegador web.

**Proceso de inscripción:**

1. Escribe tu nombre completo
2. Selecciona de las sugerencias que aparecen
3. Verifica tus datos (nivel, curso, paralelo)
4. Elige tu materia optativa
5. Confirma la inscripción
6. Recibe confirmación visual

### Acceso administrativo

```
https://github.com/slddarias036-art/Opt_2526/blob/main/admin.html
```

Abrir el archivo `admin.html` desde el navegador web.

**Proceso administrativo:**

1. Ingresa la URL del Google Apps Script (proporcionada por el desarrollador)
2. Haz clic en "Ver inscripciones"
3. Visualiza:
   - Cupos por materia (Bachillerato)
   - Cupos por materia (Educación Básica)
   - Listado completo de inscritos
4. Exporta datos a CSV si lo necesitas

---

## Estructura del repositorio

```
Opt_2526/
├── index.html              # Interfaz de inscripción para estudiantes
├── admin.html              # Panel administrativo
└── README.md               # Documentación del proyecto
```

### Tamaño y composición
- **Tamaño total:** ~70 KB
- **Lenguaje:** 100% HTML5 (CSS y JavaScript incrustados)
- **Archivos:** 2 páginas estáticas

---

## Configuración requerida

### 1. URL del Google Apps Script

El sistema requiere una URL de backend configurada en ambos archivos:

**En `index.html` (línea 175):**
```javascript
const API_URL = "https://script.google.com/macros/s/AKfycbx4DxNzsHBOvOs5N8l-FQArV35e_EJ25aSMopSKbtL4md-sXSszCc1-aDGrw4XKGStTBw/exec";
```

### 2. Lista de estudiantes

La lista oficial de estudiantes se pre-carga en `index.html` (línea 178):

```javascript
const ESTUDIANTES = [
  {"nombre": "AMANGANDI PILAMUNGA KAROL ARIANA", "curso": "8vo", "paralelo": "A", "nivel": "Basica"},
  {"nombre": "BARRIONUEVO GALEAS LUISANA VALENTINA", "curso": "8vo", "paralelo": "B", "nivel": "Basica"},
  // ... más estudiantes
];
```

### 3. Materias optativas

Configuradas por nivel en `index.html` (línea 180):

```javascript
const MATERIAS = {
  Bachillerato: ["Personal Branding", "Video Mapping", "Programación No-Code", "Diseño Gráfico"],
  Basica: ["Multimedia y Creación de Contenido", "Robótica e IA", "Personal Branding", "Producción Musical"]
};
```

### 4. Cupo máximo por materia

```javascript
const CUPO_MAX = 25;  // línea 184
```

---

## Especificaciones técnicas

### Dependencias
- Ninguna (vanilla JavaScript, sin librerías externas)

### Navegadores compatibles
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Cualquier navegador con soporte ES6

### Conexión a Internet
- Requerida para sincronización de cupos
- Requerida para envío de inscripciones

### Almacenamiento
- LocalStorage: para recordar URL de API en panel administrativo

---

## Funcionalidades detalladas

### Inscripción de estudiantes

| Función | Descripción |
|---------|-------------|
| **Autocompletado** | Búsqueda con filtrado en tiempo real |
| **Validación** | Verificación contra lista oficial |
| **Control de cupos** | Bloqueo de materias llenas |
| **Estado de cupos** | Indicador visual (disponibles/pocos/lleno) |
| **Confirmación** | Resumen visual de inscripción completada |
| **Reinicio** | Botón para inscribir a otro estudiante |

### Panel administrativo

| Función | Descripción |
|---------|-------------|
| **Resumen de cupos** | Visualización de usados vs. máximo |
| **Separación por nivel** | Vistas independientes (Bachillerato/Básica) |
| **Tabla de inscritos** | Scroll con historial completo |
| **Exportación CSV** | Descarga para análisis en Excel/Sheets |
| **Auto-refresco** | Actualización cada 20 segundos |

---

## Requisitos de implementación

✅ **Completado en este repositorio:**
- Interfaz HTML5 responsiva
- Lógica de validación en cliente
- Autocompletado de estudiantes
- Control visual de cupos
- Panel administrativo

⚠️ **Requiere implementación externa (Google Apps Script):**
- Base de datos de inscripciones
- Validación de cupos en servidor
- Persistencia en Google Sheets
- Endpoint REST para consultas

---

## Notas de seguridad

- La lista de estudiantes se carga en el cliente (visible en el navegador)
- La validación principal debe ocurrir en el backend
- Se recomienda proteger la URL de API con autenticación adicional
- Los datos sensibles nunca se deben hardcodear en el HTML

---

## Autor / Responsable

**Institución:** Eight Academy  
**Período:** 2026-2027  
**Mantenedor del repositorio:** slddarias036-art

---

## Enlaces de acceso

| Recurso | Enlace |
|---------|--------|
| **Repositorio GitHub** | [slddarias036-art/Opt_2526](https://github.com/slddarias036-art/Opt_2526) |
| **Interfaz de estudiantes** | [Abrir index.html](https://github.com/slddarias036-art/Opt_2526/blob/main/index.html) |
| **Panel administrativo** | [Abrir admin.html](https://github.com/slddarias036-art/Opt_2526/blob/main/admin.html) |
| **Ver código fuente** | [Explore repository](https://github.com/slddarias036-art/Opt_2526) |

---

## Resumen

Este sistema automatiza completamente el proceso de inscripción de materias optativas, eliminando tramitología manual y proporcionando visibilidad en tiempo real sobre el estado de cupos. La arquitectura desacoplada permite mantener la interfaz independiente de los cambios en el backend, facilitando actualizaciones futuras y reutilización en otros períodos académicos.
