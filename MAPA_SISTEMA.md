# SIREPI v5 — Mapa de Funciones del Sistema
## Referencia para revisión y corrección modular

---

## PROTOCOLO DE TRABAJO

Cuando hay un problema, reportar así:
> "En **[página]** al hacer **[acción]**, ocurre **[error o comportamiento]**"

Ejemplo:
> "En **/registro** al guardar un nuevo asistente, muestra error 500"

Con eso se localiza exactamente **qué función** y **qué líneas** tocar,
y se aplica solo el parche necesario sin tocar el resto del sistema.

---

## MÓDULOS Y FUNCIONES

### BLOQUE A — Autenticación
**Archivo:** `modulos/auth.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `login()` | GET /login | L15 | Muestra pantalla de login |
| `api_login()` | POST /api/login | L22 | Valida usuario y contraseña, inicia sesión |
| `logout()` | GET /logout | L43 | Cierra sesión y redirige |

**Template:** `templates/login.html` (95 líneas)

---

### BLOQUE B — Registro en Piso
**Archivo:** `modulos/registro.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `registro()` | GET /registro | L19 | Muestra formulario dinámico |
| `api_guardar()` → `registro_piso` | POST /api/guardar | L28 | Guarda nuevo asistente |
| `api_guardar()` → `agrega_hijo` | POST /api/guardar | L28 | Agrega acompañante |
| `api_guardar()` → `nuevo_usuario` | POST /api/guardar | L28 | Crea usuario (admin) |
| `api_guardar()` → `parametros` | POST /api/guardar | L28 | Guarda parámetros |

**Template:** `templates/registro.html` (184 líneas)
- Bloque principal: campos dinámicos desde `form_campos`
- Sidebar: últimas capturas (refresco cada 15s)

---

### BLOQUE C — Listado y Detalle
**Archivo:** `modulos/listado.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `registros()` | GET /registros | L17 | Página de listado |
| `api_registros()` | GET /api/registros | L24 | JSON con todos los registros activos |
| `detalle()` | GET /detalle/<id> | L65 | Ficha completa con formulario editable + preview gafete |
| `api_modificar()` → `confirmar` | POST /api/modificar | L151 | Confirma/desconfirma asistente |
| `api_modificar()` → `modifica_registro` | POST /api/modificar | L151 | Edita datos (solo admin) |
| `api_eliminar()` | POST /api/eliminar | L241 | Elimina registro o usuario (solo admin) |
| `api_confirmar_al_imprimir()` | POST /api/confirmar_al_imprimir | L268 | Confirma automáticamente al imprimir gafete |

**Templates:**
- `templates/registros.html` (141 líneas) — tabla dinámica + acciones inline
- `templates/detalle.html` (326 líneas) — formulario + preview live del gafete

---

### BLOQUE D — Pistoleo QR
**Archivo:** `modulos/pistoleo.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `pistoleo()` | GET /pistoleo | L16 | Pantalla de escaneo |
| `api_pistoleo()` | POST /api/pistoleo | L25 | Procesa QR, valida acceso, registra historial |

**Templates:**
- `templates/pistoleo.html` (98 líneas) — input QR + historial de capturas
- `templates/pistoleo_ok.html` (30 líneas) — respuesta acceso permitido
- `templates/pistoleo_error.html` (38 líneas) — respuesta acceso denegado

---

### BLOQUE E — Gafete
**Archivo:** `modulos/gafete.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `gafete()` | GET /gafete/<id> | L20 | Página de impresión optimizada |
| `gafete_config()` | GET /gafete-config | L36 | Panel de configuración (admin) |
| `api_gafete_config_guardar()` | POST /api/gafete-config/guardar | L43 | Guarda configuración del gafete |

**Templates:**
- `templates/gafete.html` (259 líneas) — layout de impresión, auto-fit nombre, QR
- `templates/gafete_config.html` (377 líneas) — panel de config con preview

---

### BLOQUE F — Importación CSV
**Archivo:** `modulos/importacion.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `importar()` | GET /importar | L19 | Página de importación con historial |
| `api_importar_preview()` | POST /api/importar/preview | L31 | Lee CSV y devuelve cabeceras + muestra |
| `api_importar_ejecutar()` | POST /api/importar/ejecutar | L52 | Ejecuta importación con mapeo de columnas |

**Template:** `templates/importar.html` (326 líneas)

---

### BLOQUE G — Constructor de Formulario
**Archivo:** `modulos/form_builder.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `form_builder()` | GET /form-builder | L21 | Panel de configuración de campos |
| `api_fb_guardar()` | POST /api/form-builder/guardar | L28 | Guarda etiquetas, orden, activo/obligatorio |
| `api_fb_nuevo()` | POST /api/form-builder/nuevo | L44 | Crea campo personalizado nuevo |
| `api_fb_eliminar()` | POST /api/form-builder/eliminar | L69 | Elimina campo personalizado |

**Template:** `templates/form_builder.html` (274 líneas)
- Drag & drop con SortableJS
- Toggles activo/obligatorio por campo
- Modal para nuevo campo

---

### BLOQUE H — Administración
**Archivo:** `modulos/admin.py`

| Función | Ruta | Línea | Descripción |
|---|---|---|---|
| `usuarios()` | GET /usuarios | L17 | Lista de usuarios del sistema |
| `parametros()` | GET /parametros | L24 | Parámetros + config de backup |
| `api_parametros_guardar()` | POST /api/parametros/guardar | L32 | Guarda parámetros y reconfigura backup en caliente |
| `reportes()` | GET /reportes | L54 | Dashboard de estadísticas |
| `api_reportes_json()` | GET /api/reportes/json | L70 | JSON con todos los registros para DataTable |
| `api_buscar()` | GET /api/buscar?q= | L86 | Búsqueda predictiva global (todos los campos) |

**Templates:**
- `templates/usuarios.html` (89 líneas)
- `templates/parametros.html` (140 líneas) — incluye bloque de backup
- `templates/reportes.html` (196 líneas) — tabla con columnas configurables

---

### BLOQUE I — Backup Automático
**Archivo:** `modulos/backup.py`

| Función | Línea | Descripción |
|---|---|---|
| `iniciar_backup()` | L59 | Lanza hilo daemon al arrancar el servidor |
| `reconfigurar()` | L79 | Cambia intervalo/ruta/estado sin reiniciar |
| `estado()` | L95 | Devuelve dict con estado actual para la UI |
| `_hacer_backup()` | L35 | Copia la BD con timestamp al directorio logs/ |
| `_loop()` | L49 | Hilo infinito que ejecuta backup cada N segundos |

---

### BLOQUE J — Helpers Compartidos
**Archivo:** `modulos/helpers.py`

| Función | Línea | Descripción |
|---|---|---|
| `get_db()` | L24 | Conexión SQLite con Row factory |
| `query_db()` | L29 | SELECT → lista de Rows (o uno con `one=True`) |
| `execute_db()` | L35 | INSERT/UPDATE/DELETE → devuelve lastrowid |
| `login_required` | L44 | Decorador: redirige a /login si no hay sesión |
| `admin_required` | L52 | Decorador: 403 si no es admin |
| `ahora()` | L63 | Devuelve (fecha_str, hora_str) del momento actual |
| `es_admin()` | L67 | True si NivelUsuario == 1 en sesión |
| `get_campos_activos()` | L70 | Campos del formulario activos ordenados |
| `get_extras_registro()` | L73 | Campos extra de un registro como dict |
| `get_params()` | L78 | Fila de parametros_sistema |

---

### BLOQUE K — Arranque
**Archivo:** `app.py`

Lee config de backup desde BD → llama `iniciar_backup()` → registra 10 Blueprints → `app.run()`

---

## BASE DE DATOS — Tablas

| Tabla | Propósito |
|---|---|
| `usuarios` | Cuentas del sistema (admin/operador) |
| `tipo_participantes` | Categorías de asistentes |
| `registro` | Registro principal de asistentes |
| `validaciones` | Historial de accesos por QR |
| `pistoleos` | Log de cada escaneo realizado |
| `parametros_sistema` | Config global + backup |
| `form_campos` | Definición del formulario dinámico |
| `registro_campos_extra` | Valores de campos personalizados |
| `importaciones` | Historial de importaciones CSV |
| `gafete_config` | Configuración del diseño del gafete |

---

## CÓMO REPORTAR UN PROBLEMA

```
Módulo:   [A/B/C/D/E/F/G/H/I/J]
Página:   /ruta
Acción:   qué hice (guardar, hacer clic en X, etc.)
Error:    mensaje exacto o descripción del comportamiento
```

Esto permite aplicar un **parche quirúrgico** en lugar de
regenerar archivos completos.
