---
name: manual-usuario
description: Genera y mantiene manuales de usuario en español para aplicaciones web, especialmente Laravel 13 y Livewire 4. Solicita datos de login cuando el flujo requiere autenticación, analiza flujos reales, documenta pasos, permisos, validaciones, errores frecuentes y usa capturas reales solo cuando exista un navegador automatizado disponible. Guarda la salida fuera del proyecto, en la carpeta manual-usuario-<nombre-proyecto>, y entrega el resultado en formato DOCX.
compatibility: opencode
metadata:
  audience: documentation
  workflow: user-manuals
  opencode/slash: "true"
  opencode/autoinvoke: "false"
---

# Skill: Manuales de usuario

## Objetivo

Crear manuales funcionales, precisos y mantenibles para usuarios finales. Prioriza lo que una persona necesita hacer en la interfaz, no una explicación del código fuente. El idioma predeterminado es español claro, formal y neutral.

Esta skill es bajo demanda. No generes ni actualices documentación durante tareas normales de desarrollo si el usuario no lo solicita.

## Modos de trabajo

Antes de crear nada, determina la carpeta de salida `manual-usuario-<nombre-proyecto>` y comprueba si ya contiene un manual previo (`manual.md` y/o `manual.docx`). Elige el modo:

- **Creación desde cero**: no existe un manual previo para ese proyecto o módulo. Sigue el flujo completo de las fases 1 a 6.
- **Actualización incremental**: ya existe un manual previo. Retómalo y actualiza únicamente las partes desactualizadas. No regeneres el manual completo ni reescribas lo que sigue siendo válido.

Reglas del modo actualización incremental:

1. Lee el manual existente antes de inspeccionar el código. Úsalo como base.
2. Compara el manual con el estado actual del código y de la interfaz. Identifica solo las diferencias: funciones nuevas, funciones eliminadas, pasos, campos, permisos, mensajes o capturas que cambiaron.
3. Modifica exclusivamente las secciones afectadas por esas diferencias. Conserva intactos el texto, el orden, los encabezados y las capturas que sigan siendo válidos.
4. Agrega secciones nuevas solo si el proyecto incorporó funciones que el manual aún no cubre.
5. Marca en la sección correspondiente o en los pendientes qué se actualizó y qué cambió.
6. Si el manual existente está completo y vigente, no lo reescribas: informa que no requiere cambios.
7. Conserva el `manual.md` fuente y regenera `manual.docx` a partir del Markdown actualizado.

Si el usuario ya indica el modo o un archivo concreto, respétalo sin preguntar.

## Versiones y revisiones

Todo manual lleva un número de versión, un número de revisión y un historial de revisiones, visibles en la portada y en el encabezado.

- Manual nuevo: Versión 1.0, Revisión 1.
- Actualización incremental: lee del manual previo la Versión y la Revisión existentes y aplícalas así:
  - Si el cambio agrega, elimina o reestructura funciones o secciones: sube la versión menor (por ejemplo, 1.0 → 1.1) y reinicia la Revisión a 1.
  - Si el cambio solo corrige o ajusta contenido existente: conserva la versión y sube la Revisión (por ejemplo, Revisión 1 → 2).
  - Si no hay cambios reales: no regeneres el manual; informa que sigue vigente.
- Si el manual previo no declara versión, asume 1.0 y Revisión 1, y anótalo.
- Registra cada cambio en la tabla "Historial de revisiones" (fecha, versión, revisión, descripción breve).
- No inventes versiones ni fechas: usa la fecha real del trabajo.

## Portada y logotipos

Genera siempre que sea posible una portada como primera página del manual (sección "Portada" de la fase 4). Incluye los logotipos y escudos oficiales del sistema, obtenidos del propio proyecto.

- Busca los recursos de marca en el proyecto (por ejemplo, `public/images`, `public/img`, `resources/images`, `assets` o carpetas similares), con nombres como `logo`, `escudo`, `isotipo`, `marca`, `simbolo`, `brand`, en formatos `.svg`, `.png`, `.jpg` o `.webp`.
- Prefiere el `.svg` o la imagen de mayor resolución. Inserta sin deformar y con tamaño legible.
- Si hay varios candidatos ambiguos, pide al usuario cuál usar. Si no encuentras ninguno, pide los archivos al usuario o deja un marcador en la portada y anótalo en pendientes.
- Nunca inventes, dibujes ni generes logotipos falsos.

## Tabla de contenido

El manual incluye una tabla de contenido (índice) que lista cada sección y cada funcionalidad documentada, con su numeración.

- Usa una jerarquía de encabezados consistente: el título general es el nivel 1 (`#`), las secciones usan nivel 2 (`##`) numerado, y las funcionalidades y subsecciones usan nivel 3 (`###`) numerado (por ejemplo, `### 4.1 Registrar una inspección`).
- Da a cada funcionalidad o procedimiento su propio encabezado con un título específico y descriptivo, no genérico, para que aparezca de forma identificable en el índice.
- Ordena el índice según el orden real del documento y ajusta la profundidad para incluir todos los niveles que el manual use.
- En Markdown, escribe la tabla de contenido como lista con enlaces a los encabezados (anclas), después de la portada y antes de la sección 1.
- En DOCX, genera el índice real con la opción de tabla de contenido del conversor (por ejemplo, `pandoc --toc --toc-depth=3`) o con el campo de índice de Word/LibreOffice. Prefiere el índice navegable generado por el conversor sobre una lista estática.
- El índice debe reflejar el contenido final: no incluyas entradas de secciones inexistentes ni omitas secciones presentes. Verifícalo justo antes de entregar.

## Reglas obligatorias

1. Inspecciona el proyecto antes de documentarlo. Identifica framework, rutas, componentes de página, vistas, formularios, permisos y flujos relacionados.
2. Documenta únicamente funciones que puedas confirmar en el código o verificar en una aplicación accesible. No inventes botones, campos, permisos, mensajes ni resultados.
3. Distingue entre comportamiento confirmado por código y comportamiento comprobado visualmente. Si algo no se puede confirmar, indícalo como pendiente de validación.
4. No modifiques código de la aplicación, datos, configuración ni permisos para producir el manual. No envíes formularios reales, no borres ni crees registros y no ejecutes acciones destructivas.
5. No intentes evadir autenticación ni controles de acceso. Cuando el flujo requiera iniciar sesión, pide siempre al usuario los datos de login (URL de acceso, usuario y contraseña de prueba) antes de continuar, y espera a recibirlos. Usa únicamente credenciales de prueba proporcionadas expresamente por el usuario y entornos autorizados. Nunca inventes credenciales, nunca las guardes en archivos y nunca las incluyas en el manual.
6. Nunca incluyas contraseñas, tokens, claves API, datos personales reales, expedientes reales ni información confidencial en capturas o manuales. Usa datos ficticios y oculta cualquier dato sensible visible.
7. No ejecutes comandos de instalación ni agregues dependencias sin autorización. No instales Playwright, MCP, Pandoc u otras herramientas por cuenta propia.
8. No afirmes que se tomó una captura si no se generó realmente. Si no hay navegador automatizado disponible, entrega el manual sin capturas y añade una lista de capturas pendientes con instrucciones para obtenerlas.
9. Genera el manual fuera de la ruta del proyecto, nunca dentro de su árbol de directorios. Usa como carpeta de salida `manual-usuario-<nombre-proyecto>`, ubicada como hermana del directorio raíz del proyecto (por ejemplo, `..\manual-usuario-<nombre-proyecto>` en Windows o `../manual-usuario-<nombre-proyecto>` en Linux/macOS). `<nombre-proyecto>` es el nombre de la carpeta raíz del proyecto o, si existe, el nombre del paquete de `composer.json`. Si el usuario indica otra carpeta, respétala siempre que esté fuera del proyecto. No generes archivos adicionales fuera de esa carpeta.
10. Antes de documentar, pregunta solo por los datos esenciales que falten: público objetivo, módulo/flujo que se documentará, entorno autorizado, datos de login (si el flujo requiere autenticación), formato final y carpeta de salida (solo si el usuario quiere una distinta a la predeterminada). Si la solicitud ya los aclara, continúa sin preguntas innecesarias.
11. Antes de crear, comprueba si ya existe un manual previo en la carpeta de salida. Si existe, retómalo y actualiza solo las partes desactualizadas (actualización incremental). Si no existe, créalo desde cero. No sobrescribas un manual vigente sin cambios.
12. Genera siempre que sea posible una portada con el nombre del sistema, sus logotipos y escudos oficiales, el módulo, la versión, la revisión, la fecha y el público objetivo. Usa solo logotipos reales del proyecto; si no hay, pídelos o deja un marcador y anótalo en pendientes.
13. Asigna automáticamente la versión y la revisión según "Versiones y revisiones", y mantén el historial de revisiones. La versión y la revisión deben aparecer en la portada y en el encabezado, y coincidir entre sí.
14. Incluye una tabla de contenido (índice) que liste cada sección y cada funcionalidad con su numeración y en el orden real del documento, con encabezados jerárquicos consistentes. En DOCX, usa el índice navegable del conversor cuando esté disponible. Verifica que el índice coincida con el contenido final.

## Flujo de trabajo

### Fase 1: delimitar el manual

Primero determina la carpeta de salida y comprueba si ya existe un manual previo (`manual.md` o `manual.docx`) para decidir entre creación desde cero o actualización incremental (ver "Modos de trabajo"). En modo actualización, lee el manual existente y úsalo como base.

Determina:
- Aplicación y módulo.
- Perfil del lector (por ejemplo, operador, administrador o ciudadano).
- Tareas que debe poder completar.
- Versión o rama documentada, si se conoce.
- Formato de salida: **DOCX** (obligatorio). Conserva el Markdown como fuente intermedia para generar el DOCX.
- Si existe una URL de pruebas accesible y un navegador automatizado configurado.
- Si el flujo requiere autenticación, solicita los datos de login de prueba (URL, usuario y contraseña) antes de avanzar.
- Carpeta de salida. Por defecto `manual-usuario-<nombre-proyecto>`, hermana del proyecto y fuera de su árbol. Confírmala si el usuario no la indicó.

Si el usuario pide un manual completo, primero inventaría los módulos detectables y presenta una propuesta de alcance antes de intentar cubrir toda la aplicación.

### Fase 2: inspeccionar el código

Para proyectos Laravel 13 + Livewire 4, revisa según corresponda:
- `routes/web.php` y archivos de rutas adicionales.
- Componentes Livewire en `app/Livewire/` y sus vistas asociadas.
- Vistas Blade en `resources/views/`.
- Middleware, Policies, Gates y comprobaciones de autorización.
- Requests, reglas de validación y mensajes visibles al usuario.
- Menús, navegación, layouts, formularios, tablas, modales y estados vacíos.
- Traducciones y mensajes de éxito/error.
- Tests existentes solo como fuente complementaria de comportamiento; no crees ni modifiques pruebas.
- Recursos de marca del sistema (logotipos y escudos) para la portada: revisa `public/images`, `public/img`, `resources/images`, `assets` o similares.

Usa búsquedas dirigidas y lee solo los archivos relevantes. No asumas que el nombre de una clase describe exactamente el comportamiento visible.

### Fase 3: verificar el flujo visual

Si el usuario proporciona una URL de un entorno de pruebas y existe Playwright MCP u otra herramienta de navegador ya configurada:
1. Confirma que el entorno es autorizado y no productivo, salvo autorización explícita.
2. Si el flujo requiere iniciar sesión, pide los datos de login al usuario y espera a recibirlos antes de navegar. Usa solo credenciales de prueba; nunca credenciales de producción y nunca las guardes en archivos.
3. Navega por el flujo de manera no destructiva.
4. Registra nombres reales de menús, botones, campos y mensajes.
5. Captura pantallas reales en los puntos que ayuden a comprender cada paso.
6. Guarda las imágenes en una carpeta `images/` dentro de la carpeta de salida, con nombres descriptivos, por ejemplo `01-listado-tramites.png`.
7. Revisa que no se vean secretos ni datos personales reales. Si no se pueden ocultar con seguridad, no incluyas la captura.
8. No simules que una operación se completó. Si el flujo requiere guardar, enviar, aprobar, pagar, eliminar o cambiar estados, detente antes de la acción y solicita autorización explícita para probarla.

Si el navegador no está disponible, no instales ni configures nada automáticamente. Continúa mediante análisis de código y enumera las capturas que el usuario debe proporcionar o habilitar.

### Fase 4: redactar el manual

En modo creación, usa la estructura siguiente. En modo actualización incremental, parte del `manual.md` existente: conserva su estructura y redacta o edita solo las secciones afectadas por las diferencias detectadas.

Usa esta estructura, adaptándola al tamaño del módulo:

### Portada (primera página)

Genera siempre que sea posible una portada como primera página del DOCX:

- Nombre del sistema o institución.
- Logotipos y escudos oficiales del sistema (uno o varios), centrados (ver "Portada y logotipos").
- Nombre del módulo documentado.
- Versión y Revisión (ver "Versiones y revisiones").
- Fecha de revisión.
- Público objetivo.

# Manual de usuario: [nombre del módulo]

- Aplicación:
- Módulo:
- Público objetivo:
- Versión: [x.y]
- Revisión: [n]
- Fecha de revisión:
- Estado de verificación: código revisado / interfaz verificada / parcialmente verificado

## Tabla de contenido
Lista navegable con la numeración de cada sección y de cada funcionalidad, en el orden real del documento (ver "Tabla de contenido"). Debe incluir todas las funcionalidades, no solo las secciones principales.

## 1. Objetivo
Explica en lenguaje sencillo para qué sirve el módulo.

## 2. Requisitos de acceso
Indica el rol o permiso necesario solo si se ha confirmado. No incluyas credenciales.

## 3. Cómo ingresar
Describe la navegación desde el menú real de la aplicación.

## 4. Procedimientos
Cada funcionalidad o tarea lleva su propio encabezado de nivel 3, numerado y con título específico, para que aparezca en la tabla de contenido:

### 4.1 [Nombre específico de la funcionalidad]
1. Propósito.
2. Pasos numerados y concretos.
3. Campos requeridos y formato esperado.
4. Validaciones y mensajes que se hayan confirmado.
5. Resultado esperado.
6. Captura real, si existe, con texto alternativo y referencia dentro del paso.

### 4.2 [Nombre específico de la siguiente funcionalidad]
Misma estructura que 4.1.

## 5. Estados, permisos y restricciones
Explica qué puede hacer cada perfil únicamente cuando esté respaldado por evidencia.

## 6. Errores frecuentes y solución
Incluye solo errores confirmados. Para problemas no verificados, etiquétalos como posibles y explica cómo confirmarlos.

## 7. Preguntas frecuentes
Incluye preguntas prácticas derivadas del flujo documentado.

## 8. Glosario
Define términos técnicos necesarios para el usuario final.

## 9. Pendientes de verificación
Lista explícitamente las dudas, pantallas inaccesibles o pasos no verificados.

## 10. Historial de revisiones
Tabla con fecha, versión, revisión y descripción breve de cada cambio. En modo actualización, conserva las filas previas y agrega una nueva fila para el cambio actual (ver "Versiones y revisiones").

## Reglas de redacción

- Escribe en español claro, formal y orientado a tareas.
- Dirígete al usuario de forma consistente; prefiere instrucciones directas: “Seleccione”, “Ingrese”, “Presione”.
- Una acción por paso. Mantén los pasos cortos.
- Usa los nombres exactos de la interfaz, entre comillas cuando sea útil.
- No expliques clases, métodos, tablas o arquitectura salvo que el público sea técnico y lo solicite.
- No uses capturas decorativas ni ilustraciones inventadas como si fueran la interfaz real.
- Numera las capturas en orden de aparición y añade una leyenda breve.
- No expongas rutas internas, nombres de servidores, IP, stack traces ni detalles sensibles.
- Incluye fecha de revisión, alcance y limitaciones.
- Mantén una jerarquía de encabezados consistente (nivel 1: título; nivel 2: secciones; nivel 3: funcionalidades) y un título específico por funcionalidad, para que la tabla de contenido sea correcta y completa.

## Fase 5: revisar calidad

Antes de entregar, verifica:
- Cada procedimiento corresponde a una función existente.
- Los pasos siguen el orden real de la interfaz.
- Los campos y permisos no fueron inferidos sin evidencia.
- Las capturas existen, corresponden a la versión documentada y no contienen información sensible.
- Las imágenes quedan embebidas en el DOCX o referenciadas con rutas relativas en el Markdown.
- El archivo `manual.docx` se generó y no está vacío.
- No hay credenciales, secretos ni datos reales sensibles.
- Las secciones pendientes están claramente marcadas.
- El manual no contradice mensajes o validaciones observados.
- No se modificó código de la aplicación ni se ejecutaron acciones destructivas.
- La salida está fuera de la ruta del proyecto, en la carpeta `manual-usuario-<nombre-proyecto>`.
- Solo se usaron credenciales de prueba solicitadas al usuario y ninguna quedó guardada en el manual ni en otros archivos.
- En modo actualización, solo cambiaron las secciones afectadas; el resto conserva el contenido y las capturas válidas previas.
- En modo actualización, los cambios realizados quedaron listados para el usuario.
- La portada incluye los logotipos/escudos reales del sistema o, si no había, un marcador anotado en pendientes.
- La versión y la revisión coinciden entre portada, encabezado e historial de revisiones, y siguen el esquema de "Versiones y revisiones".
- La tabla de contenido lista todas las secciones y funcionalidades con su numeración correcta, en el orden del documento, sin entradas de más ni de menos.
- Los encabezados siguen una jerarquía consistente y cada funcionalidad tiene su propio título específico.

## Fase 6: salida

- Genera el archivo final `manual.docx` en la carpeta de salida. El DOCX es el entregable obligatorio. Mantén la salida fuera de la ruta del proyecto (carpeta por defecto `manual-usuario-<nombre-proyecto>`, hermana del directorio raíz). Nunca escribas la salida dentro del proyecto.
- Conserva el `manual.md` fuente en la misma carpeta de salida. En modo actualización, edita ese archivo en su lugar y regenera el DOCX a partir de él.
- Si hay capturas reales, guárdalas en `images/` dentro de esa carpeta e insértalas en el DOCX, numeradas y con leyenda breve.
- Embebe en el DOCX los logotipos y escudos de la portada; guárdalos también en `images/` si el Markdown debe referenciarlos con rutas relativas. No alteres su contenido ni su proporción.
- Convierte el Markdown a DOCX con una herramienta ya instalada (por ejemplo `pandoc manual.md --toc --toc-depth=3 -o manual.docx` o `libreoffice --headless --convert-to docx manual.md`). Añade la tabla de contenido al generar el DOCX (por ejemplo, `--toc --toc-depth=3` en Pandoc). Verifica que `manual.docx` exista, no esté vacío y contenga el índice. No instales herramientas por tu cuenta.
- Si no hay conversor disponible, entrega `manual.md`, indica que falta la conversión a DOCX e incluye el comando exacto para generarlo con la herramienta disponible.
- Para manuales grandes, crea un archivo por módulo únicamente si el usuario lo solicita o aprueba el alcance.
- Al finalizar, informa qué se documentó, qué se verificó visualmente, qué archivos se generaron (con la ruta completa de la carpeta de salida) y qué queda pendiente.

## Invocación

Esta skill se puede invocar desde OpenCode con `/manual-usuario` o mencionando `@manual-usuario`, según la versión y configuración instaladas.

Ejemplos:
- “@manual-usuario documenta el módulo de activos para el perfil de administrador. Genera el DOCX; no modifiques la aplicación.”
- “/manual-usuario crea el manual del flujo de creación de trámites. Usa solo capturas reales del entorno de pruebas.”
- “Actualiza el manual del módulo de inspecciones comparando el código actual con `../manual-usuario-<nombre-proyecto>/manual.md`. Modifica únicamente los archivos del manual.”
