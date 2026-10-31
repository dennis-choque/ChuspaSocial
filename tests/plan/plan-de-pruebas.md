# Plan de Pruebas

## 1. Objetivo

Verificar que el plugin `local_chuspasocial` para Moodle 4.5 LTS cumple sus requisitos funcionales para el release v0.1.0: muro social general y por materia, comentarios y reacciones, etiquetas, seguimiento de usuarios, notificaciones, reporte de contenido inapropiado y marketplace de libros, respetando los permisos y la visibilidad de cada funcionalidad.

Las pruebas se centrarán especialmente en cuatro componentes: muro social, comentarios y reacciones, marketplace de libros y eventos del plugin e integración con Moodle. También se comprobarán las dependencias, la integridad de los datos y las condiciones de acceso de cada funcionalidad.

Se verificará la compatibilidad declarada con PHP 8.1 a 8.3, MariaDB, MySQL y PostgreSQL, así como la disponibilidad de la interfaz en español e inglés.

## 2. Alcance

### En alcance

**1. Muro social**

- Visualización del muro general y de las publicaciones disponibles, mostrando primero las más recientes.
- Creación de publicaciones con texto e imágenes.
- Publicación y visualización de contenido en el muro de una materia.
- Restricción de acceso al muro de una materia según la inscripción y los permisos del usuario.
- Eliminación de publicaciones propias y restricción de eliminación de publicaciones ajenas.
- Validación de publicaciones vacías y del contenido ingresado.
- Validación de formatos y cantidad de imágenes adjuntas.
- Visualización segura del contenido, incluyendo enlaces y formatos de texto permitidos.
- Carga progresiva de publicaciones mediante paginación.
- Aplicación de los límites administrativos de longitud de publicaciones, cantidad de imágenes y publicaciones mostradas por página.
- Visualización de anuncios oficiales en los muros correspondientes.

**2. Comentarios y reacciones**

- Creación de comentarios asociados a una publicación.
- Visualización de los comentarios correspondientes a cada publicación.
- Ordenamiento cronológico de los comentarios.
- Eliminación de comentarios propios y restricción de eliminación de comentarios ajenos.
- Actualización del contador de comentarios.
- Registro de reacciones de los usuarios sobre las publicaciones.
- Prevención de registros duplicados de una misma reacción por usuario.
- Visualización del número total de reacciones de cada publicación.
- Restricción de comentarios y reacciones sobre publicaciones a las que el usuario no tiene acceso.

**3. Marketplace de libros**

- Creación de anuncios con título, autor, descripción, precio y fotografías.
- Asociación de anuncios con una o varias materias.
- Visualización de libros correspondientes a las materias en las que está inscrito el usuario.
- Búsqueda de libros por título y autor.
- Validación de campos obligatorios y precios mayores o iguales a cero.
- Contacto con el vendedor mediante la mensajería de Moodle.
- Visualización de un contacto externo opcional cuando haya sido registrado por el vendedor.
- Edición y eliminación de anuncios propios.
- Cambio del estado de un anuncio a vendido.
- Validación de fotografías y límites de archivos permitidos.
- Restricción de modificación y eliminación de anuncios pertenecientes a otros usuarios.
- Activación y desactivación del Marketplace según la configuración administrativa.

**4. Eventos e integración con Moodle**

Se evaluarán los eventos propios del plugin y los observadores registrados en el sistema de eventos de Moodle.

Eventos propios del plugin:

| Evento | Comportamiento que se evaluará |
|---|---|
| `post_created` | Generación del evento al crear una publicación. |
| `post_deleted` | Generación del evento al eliminar una publicación. |
| `comment_created` | Generación del evento al crear un comentario. |
| `reaction_added` | Generación del evento al registrar una reacción. |
| `listing_created` | Generación del evento al crear un anuncio de libro. |

Se comprobará que los eventos se generen con la información correspondiente a cada operación y mantengan la asociación correcta con el registro y el usuario involucrado.

Observadores de eventos de Moodle:

| Evento de Moodle | Observador | Comportamiento que se evaluará |
|---|---|---|
| `core\event\user_graded` | `observer::user_graded` | Envío de una notificación privada al estudiante cuando se publica una calificación final visible. |
| `core\event\course_module_created` | `observer::assign_created` | Creación de una publicación de actividad en el muro de la materia cuando se registra una tarea visible. |

Se verificará que:

- Las notificaciones de calificaciones se dirijan únicamente al estudiante correspondiente.
- No se envíen notificaciones cuando la calificación no sea visible.
- La creación de publicaciones de actividades se limite a las tareas (`assign`).
- No se generen publicaciones para actividades inexistentes u ocultas.
- No se generen publicaciones duplicadas para una misma tarea.
- Las publicaciones generadas mantengan su asociación con el curso correspondiente.

Archivos de referencia:
- `src/db/events.php`
- `src/classes/event/`
- `src/classes/local/observer.php`
- `src/tests/event/`

**Otras funcionalidades incluidas**

- Tags / hashtags: etiquetar publicaciones, seguir temas y filtrar el muro.
- Seguimiento de otros usuarios y visualización de perfiles sociales.
- Noticias según tags, incluyendo publicaciones de usuarios, anuncios oficiales y actividad de Moodle.
- Notificaciones por comentarios y nuevos seguidores.
- Reporte de contenido inapropiado y las operaciones de moderación contempladas en los requisitos.
- Funciones externas (AJAX) del plugin `local_chuspasocial`.
- Validación de permisos y capacidades de acceso a cada funcionalidad.
- Compatibilidad declarada con Moodle 4.5 LTS, PHP 8.1 a 8.3 y MariaDB, MySQL y PostgreSQL.
- Interfaz en español e inglés (`lang/es`, `lang/en`).

### Fuera de alcance

- Pago y entrega física de los libros: se acuerdan fuera de la plataforma.
- Pasarelas de pago y gestión de transacciones económicas.
- Funcionamiento interno del núcleo de Moodle y de sus plugins de terceros, excepto los puntos de integración utilizados por ChuspaSocial.
- Versiones de Moodle distintas de 4.5 LTS.
- Aplicaciones móviles nativas (la evaluación se limita a la interfaz web).
- Pruebas de usabilidad con usuarios reales y auditorías avanzadas de accesibilidad.
- Pruebas de estrés y carga masiva.
- Auditorías externas de seguridad y pruebas de penetración independientes.

## 3. Tipos de prueba

- [ ] Unitarias
- [ ] Integración
- [ ] Funcionales manuales
- [ ] Rendimiento
- [ ] Seguridad
- [ ] Regresión

## 4. Entornos

| Entorno | SO | Versión |
|---|---|---|
| Local | | |
| CI | Ubuntu latest | |

## 5. Responsables

| Rol | Responsable |
|---|---|
| Diseño de casos | |
| Ejecución manual | |
| Automatización | |
| Reporte | |

## 6. Criterios de salida

- [ ] Cobertura mínima del 70 % del código ubicado en `classes/`, conforme al requisito RNF-012.
- [ ] Cero bugs críticos abiertos.
- [ ] Todos los casos ejecutados.

## 7. Riesgos

| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| Incompatibilidad con Moodle 4.5 LTS o con versiones de PHP (8.1 a 8.3) | Media | Alta | Ejecutar CI con `moodle-plugin-ci` y verificar la compatibilidad sobre `MOODLE_405_STABLE` y las versiones de PHP contempladas. |
| Fallos de esquema o consultas en distintos motores de base de datos (MariaDB, MySQL, PostgreSQL) | Media | Alta | Validar el esquema XMLDB y las consultas DML en los motores de base de datos utilizados en las pruebas. |
| Fuga de publicaciones entre materias (un muro de curso lo deben ver solo los inscritos) | Media | Alta | Ejecutar casos de prueba de permisos y capacidades por curso con usuarios inscritos y no inscritos. |
| Exposición de datos personales en perfiles, seguidores o mensajería | Media | Alta | Revisar las capacidades de acceso y los mecanismos de privacidad del plugin. |
| Vulnerabilidades web (XSS en publicaciones y comentarios, CSRF y subida de imágenes) | Media | Alta | Limpiar y validar entradas, comprobar los controles de sesión y permisos, y probar archivos y contenidos maliciosos. |
| Contenido inapropiado o abuso en publicaciones y comentarios | Alta | Media | Verificar los flujos de reporte, revisión y moderación contemplados en los requisitos. |
| Errores en la generación o ejecución de eventos propios y observadores de Moodle | Media | Alta | Probar la generación de eventos, las condiciones de ejecución de los observadores y la asociación correcta de los registros. |
| Datos inconsistentes en el marketplace (precio inválido, contacto externo incorrecto) | Media | Media | Ejecutar pruebas de validación de anuncios, precios, campos requeridos y datos de contacto. |
| Bajo rendimiento del muro con muchas publicaciones, tags y notificaciones | Media | Media | Utilizar paginación, revisar consultas y comprobar los tiempos de carga con datos de ejemplo. |
| Cobertura de pruebas insuficiente (PHPUnit y Behat) por tiempo y tamaño del equipo | Alta | Media | Priorizar los casos críticos y revisar el cumplimiento del porcentaje de cobertura establecido en CI. |
| Diferencias entre entornos locales y CI (Windows, Linux, Docker) | Media | Baja | Documentar la instalación, fijar versiones de dependencias y utilizar `moodle-docker` como alternativa. |
| Fallos en las dependencias de Moodle, como cron, mensajería o tags | Media | Media | Comprobar la configuración de los servicios requeridos antes de ejecutar las pruebas de notificaciones, contacto con vendedores y etiquetado. |

## 8. Dependencias

Para ejecutar las pruebas de los componentes incluidos en el release v0.1.0 se requieren las siguientes dependencias:

| Dependencia | Descripción |
|---|---|
| Moodle 4.5 LTS | Instalación funcional sobre la rama `MOODLE_405_STABLE`. |
| Plugin ChuspaSocial | Plugin `local_chuspasocial` instalado y configurado en Moodle. |
| PHP | Versiones 8.1 a 8.3, conforme a la compatibilidad declarada en los requisitos. |
| Base de datos | MariaDB, MySQL o PostgreSQL compatibles con Moodle 4.5 LTS. |
| PHPUnit | Ejecución de pruebas unitarias y de integración. |
| Behat | Automatización de pruebas funcionales mediante navegador. |
| moodle-plugin-ci | Herramienta de validación automatizada del plugin. |
| GitHub Actions | Ejecución de los procesos de integración continua. |
| Datos de prueba | Usuarios, cursos, matrículas, publicaciones, comentarios, reacciones, anuncios y calificaciones. |
| Permisos de Moodle | Roles, capacidades y matrículas configuradas para verificar accesos permitidos y restringidos. |
| Cron de Moodle | Debe estar activo y ejecutarse periódicamente para procesar las notificaciones y tareas programadas de ChuspaSocial. |
| Mensajería de Moodle | Debe estar habilitada mediante el ajuste `messaging` para comprobar el contacto con vendedores y las notificaciones correspondientes. |
| Tags de Moodle | Deben estar habilitados mediante el ajuste `usetags` para comprobar el etiquetado y filtrado de publicaciones. |
| Servicios de Moodle | Disponibilidad de los servicios de base de datos, eventos y mensajería utilizados por los componentes evaluados. |

Las pruebas deberán ejecutarse en entornos controlados y con datos de prueba, evitando utilizar información personal o registros de producción.
