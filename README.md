# manual-usuario

Skill de OpenCode para generar y mantener manuales de usuario en español, orientados a tareas y basados en evidencia. Pensada para aplicaciones web, en especial **Laravel 13 + Livewire 4**.

El entregable final es un **DOCX**. El Markdown se usa solo como fuente intermedia.

## Qué hace

- Inspecciona rutas, componentes Livewire, vistas Blade, permisos (Policies/Gates), validaciones y traducciones antes de documentar.
- Documenta únicamente funciones confirmadas en código o verificadas en una interfaz accesible.
- Verifica flujos visuales con navegador automatizado solo si ya está configurado (Playwright MCP u otro); usa capturas reales, nunca inventadas.
- Solicita los datos de login de prueba (URL, usuario y contraseña) cuando el flujo requiere autenticación, y nunca los guarda ni los incluye en el manual.
- Genera la salida **fuera del proyecto**, en una carpeta hermana llamada `manual-usuario-<nombre-proyecto>`.
- **Retoma el manual existente**: si ya existe un manual previo en la carpeta de salida, lo actualiza de forma incremental (solo las partes desactualizadas); si no existe, lo crea desde cero.
- Indica el **nombre de la plataforma** en la portada, el encabezado y las propiedades del DOCX.
- Genera, siempre que sea posible, una **portada** con el nombre del sistema y sus **logotipos y escudos reales** (tomados del proyecto), sin inventar imágenes.
- Asigna de forma **automática la versión y la revisión** según los cambios, y mantiene un historial de revisiones.
- Genera una **tabla de contenido** correcta, con la numeración de cada sección y de cada funcionalidad documentada.
- Nunca modifica el código de la aplicación, ni ejecuta acciones destructivas, ni incluye credenciales, secretos o datos personales reales.

Es una skill **bajo demanda**: no se activa durante el trabajo normal de desarrollo.

## Cómo funciona

1. **Delimitar**: confirma el nombre de la plataforma, aplicación, módulo, público, entorno autorizado, datos de login (si aplican), carpeta de salida y formato. Detecta si ya existe un manual previo para elegir entre creación desde cero o actualización incremental.
2. **Inspeccionar el código**: revisa rutas, componentes, vistas, middleware, Policies/Gates, Requests/validaciones, menús, traducciones y los logotipos/escudos del sistema. En modo actualización, compara contra el manual existente y detecta solo las diferencias.
3. **Verificar el flujo visual** (si hay URL de pruebas y navegador): pide login, navega sin acciones destructivas y captura pantallas reales.
4. **Redactar**: crea la portada con logotipos/escudos, versión y revisión, la tabla de contenido, y estructura en objetivo, requisitos de acceso, cómo ingresar, procedimientos (uno por funcionalidad), permisos, errores, FAQ, glosario, pendientes e historial de revisiones.
5. **Revisar calidad**: comprueba que cada paso exista en el código o interfaz, sin credenciales ni datos sensibles.
6. **Salida**: genera `manual.md` y lo convierte a `manual.docx`.

## Estructura

```
.opencode/
└── skills/
    └── manual-usuario/
        └── SKILL.md      # instrucciones de la skill
```

## Instalación

La skill se resuelve desde un directorio `skills/`. Elige un ámbito:

### Global (todos tus proyectos)

```powershell
New-Item -ItemType Directory -Force "$HOME\.config\opencode\skills" | Out-Null
Copy-Item ".opencode\skills\manual-usuario" "$HOME\.config\opencode\skills\manual-usuario" -Recurse -Force
```

### Por proyecto

Clona o copia `.opencode/skills/manual-usuario` dentro del `.opencode/skills/` del proyecto destino.

Reinicia OpenCode para que detecte la skill.

## Uso

```
/manual-usuario
@manual-usuario
```

Ejemplos:

- `@manual-usuario documenta el módulo de activos para el perfil de administrador. Genera el DOCX; no modifiques la aplicación.`
- `/manual-usuario crea el manual del flujo de creación de trámites. Usa solo capturas reales del entorno de pruebas.`
- `Actualiza el manual del módulo de inspecciones comparando el código actual con ../manual-usuario-mi-proyecto/manual.md.`

## Salida

La salida se genera **fuera de la ruta del proyecto**, en una carpeta hermana llamada `manual-usuario-<nombre-proyecto>`:

```
<directorio-padre>/
├── mi-proyecto/                # el proyecto
└── manual-usuario-mi-proyecto/ # salida del manual
    ├── manual.md               # fuente Markdown (intermedia)
    ├── manual.docx             # entregable final (obligatorio)
    └── images/                 # capturas reales embebidas en el DOCX
```

`<nombre-proyecto>` es el nombre de la carpeta raíz del proyecto o el nombre del paquete de `composer.json`.

## Conversión a DOCX

El DOCX es obligatorio. La skill usa un conversor ya instalado, por ejemplo:

```bash
pandoc manual.md --toc --toc-depth=3 --metadata title="Manual de usuario - [plataforma]" -o manual.docx
# o
libreoffice --headless --convert-to docx manual.md
```

La tabla de contenido del DOCX se genera al convertir (`--toc` en Pandoc, o el campo de índice de Word/LibreOffice). En el Markdown queda como lista de enlaces a los encabezados. `--metadata title` fija el nombre de la plataforma en las propiedades del documento.

Si no hay conversor disponible, entrega `manual.md` e indica el comando exacto para generar el DOCX. La skill no instala herramientas por su cuenta.

## Requisitos

- OpenCode con soporte de skills.
- Necesario para el entregable: conversor de Markdown a DOCX instalado (Pandoc, LibreOffice u otro).
- Opcional: navegador automatizado (Playwright MCP u otro) para verificación visual y capturas.

## Licencia

[MIT](./LICENSE) © 2026 Eli Cordero
