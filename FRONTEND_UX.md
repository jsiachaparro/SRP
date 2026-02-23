# SIREPI v5 — Parámetros de Frontend / UX

Todo el look & feel del sistema se controla desde **una sola sección**
al inicio de `templates/base.html` (líneas 10–60).
Cambiar un valor ahí lo propaga a **todas las páginas** automáticamente.

---

## 1. VARIABLES CSS — El sistema de diseño

### 🎨 Colores (modo claro — `:root`)

| Variable | Valor actual | Qué controla |
|---|---|---|
| `--bg` | `#f5f0eb` | Fondo general de la página |
| `--surface` | `#faf7f4` | Fondo de panels, navbar, modales |
| `--surface2` | `#f0ebe4` | Fondo alterno (filas pares, footers) |
| `--border` | `#e8e0d8` | Bordes de panels, inputs, separadores |
| `--text` | `#1a1612` | Texto principal |
| `--text2` | `#5c5248` | Texto secundario (labels, subtítulos) |
| `--text3` | `#9e9188` | Texto tenue (placeholders, ayuda) |
| `--accent` | `#c96442` | Color principal — botones, links, iconos activos |
| `--accent2` | `#a84e30` | Hover del color principal |
| `--accent-bg` | `#fdf0eb` | Fondo tenue del color principal |
| `--green` | `#3d7a5c` | Éxito, confirmado, badge verde |
| `--green-bg` | `#eaf4ef` | Fondo de alertas verdes |
| `--red` | `#b84040` | Error, peligro, eliminar |
| `--red-bg` | `#fdf0f0` | Fondo de alertas rojas |
| `--blue` | `#3a6ea8` | Info, badges azules |
| `--blue-bg` | `#edf3fb` | Fondo de alertas azules |

### 🌙 Colores (modo oscuro — `[data-theme="dark"]`)

Mismas variables pero con valores oscuros.
Se activan automáticamente al hacer clic en el botón 🌙 del navbar.

| Variable | Valor actual |
|---|---|
| `--bg` | `#111116` |
| `--surface` | `#1c1c24` |
| `--surface2` | `#25252f` |
| `--border` | `#32323e` |
| `--text` | `#ededf2` |
| `--accent` | `#e07a54` |

---

### 📐 Geometría y tipografía

| Variable | Valor actual | Qué controla |
|---|---|---|
| `--radius` | `12px` | Radio de bordes — panels, modales, dropdowns |
| `--radius-sm` | `7px` | Radio pequeño — botones, inputs |
| `--shadow` | `0 2px 8px ...` | Sombra de panels y modales |
| `--shadow-sm` | `0 1px 3px ...` | Sombra ligera — navbar |
| `--nav-h` | `58px` | Altura de la barra de navegación |
| `--font` | `'Inter', system-ui` | Fuente de todo el sistema |
| `--fs-base` | `15px` | Tamaño de fuente base del body |

---

## 2. CÓMO CAMBIAR EL COLOR PRINCIPAL (accent)

Para cambiar de naranja-café a, por ejemplo, **azul**:

```css
/* En base.html, sección :root */
--accent:    #2563eb;   /* azul principal */
--accent2:   #1d4ed8;   /* hover */
--accent-bg: #eff6ff;   /* fondo tenue */

/* En [data-theme="dark"] */
--accent:    #60a5fa;
--accent2:   #3b82f6;
--accent-bg: #1e3a5f;
```

Eso cambia automáticamente: navbar, botones primarios, links,
buscador activo, badges, avatares, blobs decorativos.

---

## 3. COMPONENTES DISPONIBLES (clases CSS)

### Botones
```html
<button class="btn btn-primary">Principal</button>
<button class="btn btn-success">Éxito / Confirmar</button>
<button class="btn btn-danger">Peligro / Eliminar</button>
<button class="btn btn-default">Secundario</button>
<button class="btn btn-info">Informativo</button>
<button class="btn btn-warning">Advertencia</button>

<!-- Tamaños -->
<button class="btn btn-primary btn-lg">Grande</button>
<button class="btn btn-primary btn-sm">Pequeño</button>
<button class="btn btn-primary btn-xs">Mini</button>
<button class="btn btn-primary btn-block">Ancho completo</button>
```

### Labels / Badges
```html
<span class="label label-success">✓ Confirmado</span>
<span class="label label-default">Pendiente</span>
<span class="label label-primary">Info</span>
<span class="label label-danger">Error</span>
<span class="label label-warning">Atención</span>
```

### Alertas
```html
<div class="alert alert-success">Operación exitosa</div>
<div class="alert alert-danger">Ocurrió un error</div>
<div class="alert alert-info">Información</div>
<div class="alert alert-warning">Advertencia</div>
```

### Panels
```html
<!-- Colores de encabezado -->
<div class="panel panel-default">...</div>   <!-- gris neutro -->
<div class="panel panel-primary">...</div>   <!-- azul -->
<div class="panel panel-success">...</div>   <!-- verde -->
<div class="panel panel-danger">...</div>    <!-- rojo -->
<div class="panel panel-warning">...</div>   <!-- naranja -->
<div class="panel panel-info">...</div>      <!-- azul claro -->

<!-- Estructura completa -->
<div class="panel panel-default">
  <div class="panel-heading"><b>Título</b></div>
  <div class="panel-body">Contenido</div>
  <div class="panel-footer">Pie</div>
</div>
```

### Formularios
```html
<div class="form-group">
  <label>Etiqueta</label>
  <input type="text" class="form-control" placeholder="...">
  <p class="help-block">Texto de ayuda</p>
</div>

<!-- Tipos disponibles en form-control -->
<input type="text">
<input type="email">
<input type="number">
<input type="password">
<input type="date">
<select class="form-control">...</select>
<textarea class="form-control" rows="3"></textarea>
```

### Grid Bootstrap 3
```html
<div class="row">
  <div class="col-md-6">mitad izquierda</div>
  <div class="col-md-6">mitad derecha</div>
</div>

<!-- Responsive: col-xs (móvil) col-sm (tablet) col-md (desktop) -->
<div class="col-xs-12 col-sm-6 col-md-4">...</div>

<!-- Clases de visibilidad -->
<span class="hidden-xs">oculto en móvil</span>
<span class="visible-xs">solo en móvil</span>
```

---

## 4. BLOQUES JINJA DISPONIBLES EN CADA TEMPLATE

Cada página puede extender `base.html` y usar estos bloques:

```html
{% extends "base.html" %}

{% block title %}Mi Página — SIREPI{% endblock %}

{% block extra_css %}
<style>
  /* CSS extra solo para esta página */
</style>
{% endblock %}

{% block content %}
  <!-- Todo el contenido va aquí -->
{% endblock %}

{% block extra_js %}
<script>
  // JS extra solo para esta página
</script>
{% endblock %}
```

---

## 5. FUNCIONES JS GLOBALES (disponibles en todas las páginas)

Declaradas en `base.html`, usables desde cualquier template:

```javascript
// Modal de mensaje
lanzaModal("Título", "Contenido HTML", 3);  // 3 = auto-cerrar en 3s

// Alerta en formulario hijo
lanzaAlertaHijo("Título", "Mensaje");

// Guardar formulario vía AJAX → respuesta va a #res
guardar("id_del_form");

// Modificar registro vía AJAX
modificar("id_del_form");

// Toggle confirmar/desconfirmar — actualiza badge y botón automáticamente
confirma(idRegistro, 1);   // confirmar
confirma(idRegistro, 0);   // desconfirmar

// Eliminar con confirmación
eliminar(id, "registro");
eliminar(id, "usuario");

// Toggle modo oscuro/claro
toggleTema();
```

---

## 6. VARIABLES DE SESIÓN disponibles en todos los templates

```jinja2
{{ session.get('Usuario') }}        {# nombre del usuario logueado #}
{{ session.get('NivelUsuario') }}   {# 1=admin, 0=operador #}
{{ session.get('ControlAcceso') }}  {# sala/área restringida #}
{{ session.get('IdUsuario') }}      {# ID numérico #}

{# Condicional admin #}
{% if session.get('NivelUsuario') == 1 %}
  <!-- solo admins ven esto -->
{% endif %}
```

---

## 7. AJUSTES RÁPIDOS MÁS COMUNES

### Cambiar tamaño de fuente global
```css
/* base.html línea 36 */
--fs-base: 16px;   /* más grande */
--fs-base: 14px;   /* más compacto */
```

### Hacer la navbar más alta o baja
```css
/* base.html línea 34 */
--nav-h: 64px;   /* más alta */
--nav-h: 48px;   /* más compacta */
```

### Quitar bordes redondeados (look más corporativo)
```css
--radius:    4px;
--radius-sm: 3px;
```

### Cambiar fuente
```css
/* base.html línea 35 — también cambiar el link de Google Fonts */
--font: 'Roboto', sans-serif;
--font: 'DM Sans', sans-serif;
--font: system-ui, sans-serif;   /* sin fuente externa */
```

### Cambiar color del tema a verde (ejemplo)
```css
--accent:    #16a34a;
--accent2:   #15803d;
--accent-bg: #f0fdf4;
```

---

## 8. DÓNDE ESTÁ CADA COSA

```
base.html  líneas 10-37  → Variables modo claro (:root)
base.html  líneas 39-58  → Variables modo oscuro ([data-theme="dark"])
base.html  líneas 63-67  → body, links globales
base.html  líneas 71-99  → navbar y toggle móvil
base.html  líneas 114-161 → buscador global
base.html  líneas 163-171 → botón modo oscuro
base.html  líneas 174-195 → page-header
base.html  líneas 197-219 → panels
base.html  líneas 221-243 → botones
base.html  líneas 245-261 → formularios
base.html  líneas 263-275 → tablas
base.html  líneas 277-285 → labels y badges
base.html  líneas 287-293 → alertas
base.html  líneas 295-307 → modales
base.html  líneas 309-326 → DataTables
base.html  líneas 334-350 → responsive móvil
```
