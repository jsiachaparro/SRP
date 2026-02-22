# SIREPI Python
Sistema de Registro en Piso — versión Python/Flask + SQLite

---

## ¿Qué necesitas tener instalado?

- **Python 3.8 o superior** (descarga en https://python.org)
- Nada más. SQLite ya viene incluido en Python.

---

## Instalación (una sola vez)

Abre una terminal en la carpeta del proyecto y ejecuta:

```bash
pip install flask
```

Luego crea la base de datos:

```bash
python init_db.py
```

Verás este mensaje:
```
✓  Base de datos creada en: database/sirepi.db

Usuarios creados:
  Usuario: admin       | Contraseña: admin1234  | Nivel: Administrador
  Usuario: operador    | Contraseña: 1234       | Nivel: Operador
  Usuario: sala_a      | Contraseña: 1234       | Nivel: Operador (Control: General)
```

---

## Iniciar el servidor

```bash
python app.py
```

Verás:
```
✓  SIREPI iniciado en  http://localhost:5000
```

Abre tu navegador en **http://localhost:5000**

---

## Acceso desde otras computadoras (terminales en red)

Mientras el servidor esté corriendo, cualquier computadora en la misma red puede acceder usando la IP de tu servidor:

```
http://192.168.X.X:5000
```

Para saber tu IP en Windows:
```
ipconfig
```
En Mac/Linux:
```
ifconfig
```

---

## Estructura del proyecto

```
sirepi_python/
│
├── app.py              ← El servidor completo (rutas y lógica)
├── init_db.py          ← Crea la base de datos (ejecutar una vez)
├── README.md           ← Este archivo
│
├── database/
│   └── sirepi.db       ← Base de datos SQLite (se crea con init_db.py)
│
├── templates/          ← Pantallas HTML
│   ├── base.html           Plantilla base (menú, estilos, scripts globales)
│   ├── login.html          Pantalla de acceso
│   ├── registro.html       Registro de asistentes en piso
│   ├── registros.html      Listado general de registros
│   ├── detalle.html        Ficha detalle y edición de un registro
│   ├── pistoleo.html       Control de acceso por escaneo
│   ├── pistoleo_ok.html    Respuesta verde (acceso permitido)
│   ├── pistoleo_error.html Respuesta roja (acceso denegado)
│   ├── usuarios.html       Gestión de usuarios
│   ├── parametros.html     Parámetros del sistema
│   └── reportes.html       Reportes y exportación CSV
│
└── static/             ← Archivos estáticos (imágenes, CSS extra)
```

---

## Módulos del sistema

| URL            | Función                                      |
|----------------|----------------------------------------------|
| /login         | Inicio de sesión                             |
| /registro      | Captura de nuevos asistentes en piso         |
| /registros     | Listado completo con búsqueda                |
| /detalle/:id   | Ver, editar, confirmar o eliminar registro   |
| /pistoleo      | Escaneo QR / código de barras para acceso    |
| /usuarios      | Crear y eliminar usuarios del sistema        |
| /parametros    | Configuración general del sistema            |
| /reportes      | Estadísticas y exportación a CSV             |

---

## Usuarios y niveles de acceso

| Nivel         | Puede hacer                                      |
|---------------|--------------------------------------------------|
| Administrador | Todo: registrar, ver, editar, eliminar, configurar |
| Operador      | Registrar y usar el pistoleo                     |

El campo **Control de Acceso** de cada usuario define qué sala puede pistolar.
Si está vacío, puede validar cualquier tipo de boleto.

---

## Cómo hacer un respaldo de los datos

La base de datos es un solo archivo:

```
database/sirepi.db
```

Cópialo a donde quieras para respaldarlo. Puedes abrirlo con:
- DB Browser for SQLite: https://sqlitebrowser.org (gratis, visual)
- Cualquier cliente SQLite

---

## Notas

- El servidor corre en modo **debug** durante desarrollo. Para producción,
  cambia `debug=True` por `debug=False` en app.py.
- La `secret_key` en app.py debe cambiarse por una cadena larga y aleatoria
  antes de usar en producción.
