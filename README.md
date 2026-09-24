# Trabajo Práctico de Pre-Evaluación: "Sistema de Control de Accesos y CRUD Seguro en PHP"

**Objetivo:** Validar la capacidad de evolucionar una aplicación web básica hacia un sistema profesional, implementando control de acceso, persistencia de estado mediante sesiones y mecanismos de seguridad contra las vulnerabilidades más comunes en aplicaciones web.

**Situación problemática:** Son contratados para desarrollar en la empresa inmobiliaria "Campos Jeppeneros" la cual les solicita un sistema que conste de una interface con validación de usuarios y niveles (admin, comun). Dicha interface web contará con la visualización de los lotes que tienen en venta. Cada lote tendrá un precio, localidad y descripción acompañado con una foto. Los usuarios comunes solo podrán visualizar la información y los administradores agregar/modificar/eliminar elementos.

---

## Módulo 1: Base de Datos y Registro Seguro de Usuarios
1. **Estructura de la Tabla:** Diseñar una tabla `usuarios` en MySQL que contenga los campos `id`, `email`, `password` y `rol` (por ejemplo: `admin` y `usuario`).
2. **Almacenamiento de Credenciales:** Implementar el script de registro evitando guardar contraseñas en texto plano. Se debe procesar la contraseña utilizando la función `password_hash()` con el algoritmo por defecto de PHP.
3. **Prevención de Inyección SQL:** Todas las consultas a la base de datos (tanto inserción como lectura) deben ejecutarse de forma obligatoria mediante **sentencias preparadas con PDO**.

---

## Módulo 2: Autenticación y Manejo de Sesiones
1. **Flujo de Login:** 
   * Consultar el usuario por su correo mediante consultas preparadas.
   * Validar la contraseña enviada por el formulario contra el hash almacenado utilizando `password_verify()`.
2. **Gestión de Sesión:**
   * Al autenticar correctamente al usuario, iniciar la sesión con `session_start()`.
   * Almacenar los datos clave del usuario (ID, email y rol) en el superglobal `$_SESSION`.
   * Implementar `session_regenerate_id(true)` inmediatamente después del login para prevenir ataques de **fijación de sesión**.
3. **Cierre de Sesión:** Crear el script `logout.php` que limpie el arreglo `$_SESSION`, destruya la sesión en el servidor y redirija al usuario al login.

---

## Módulo 3: Autorización y Guardianes (Guards / Middleware)
1. **Modulo de Protección (`auth.php`):** Desarrollar un archivo centralizado con funciones reutilizables:
   * `esta_autenticado()`: Comprueba si existe la sesión activa.
   * `requerir_autenticacion()`: Redirige a `login.php` si el usuario no ha iniciado sesión.
   * `requerir_rol($rol_esperado)`: Verifica si el usuario posee el rol adecuado para realizar la acción. Si no tiene los permisos suficientes, debe detener la ejecución y retornar un encabezado de respuesta HTTP `403 Forbidden`.
2. **Protección del CRUD:** Aplicar los guardianes en las páginas del panel de administración para restringir las acciones de alta, baja y modificación de datos únicamente a usuarios autorizados.

---

## Módulo 4: Controles Integrales de Seguridad
1. **Saneamiento y Validación de Entradas:** Filtrar los datos recibidos mediante formularios.
2. **Mitigación de XSS (Cross-Site Scripting):** Toda salida de datos hacia el HTML (como nombres de usuario o registros cargados desde la base de datos) debe sanitizarse usando `htmlspecialchars()` antes de ser renderizada en el navegador.

---

## Criterios de Evaluación y Preguntas Teóricas

| Criterio | Descripción |
| :--- | :--- |
| **Seguridad en Contraseñas** | Correcto uso de `password_hash()` y `password_verify()` sin texto plano. |
| **Sesiones y Estado** | Manejo de `$_SESSION`, regeneración de ID y destrucción limpia al salir. |
| **Protección contra Inyecciones** | Ausencia total de consultas concatenadas directamente en SQL (uso de PDO). |
| **Saneamiento e Integridad** | Implementación de `htmlspecialchars()`, validación de formularios |


