# Dev Portfolio

Portafolio personal para presentar proyectos en desarrollo, su estatus de avance, stack técnico e imágenes. Diseño negro + oro, animaciones suaves, dinámico desde JSON.

---

## Estructura del proyecto

```
portafolio/
├── index.html              ← vista principal (no tocar estructura, solo estilo si quieres)
├── data/
│   └── projects.json       ← aquí editas TUS proyectos
├── images/                 ← aquí metes las capturas de pantalla
│   ├── placeholder-1.svg   ← placeholders, reemplazar por capturas reales
│   └── ...
├── projects/               ← opcional: aquí pones los `.portfolio.md` de cada proyecto
└── README.md
```

---

## Cómo actualizar el portafolio

### 1. Editar `data/projects.json`

Cada proyecto es un objeto con esta estructura. **Todos los campos son opcionales excepto `id`, `title`, `status`, `progress` y `desc`**:

```json
{
  "id": 1,
  "slug": "mi-proyecto",
  "title": "Nombre del Proyecto",
  "subtitle": "Full-Stack · Web",
  "status": "dev",
  "statusLabel": "En Desarrollo",
  "progress": 65,
  "eta": "3 semanas",
  "priority": "alta",
  "type": "Aplicación web",
  "role": "Tech Lead",
  "team": "4 personas",
  "client": "Área de operaciones",
  "startDate": "2025-10-01",
  "estimatedEnd": "2026-05-10",
  "desc": "Descripción corta del proyecto.",
  "current": "Qué estoy haciendo ahora mismo.",
  "missing": "Qué falta por hacer.",
  "stack": {
    "frontend": ["Next.js", "TypeScript"],
    "backend": ["Node.js", "Express"],
    "database": ["PostgreSQL"],
    "infra": ["Docker", "AWS"],
    "otros": ["Jest"]
  },
  "images": [
    { "src": "images/mi-proyecto-1.png", "caption": "Dashboard" }
  ],
  "milestones": [
    { "label": "Fase 1", "done": true },
    { "label": "Fase 2", "done": false, "wip": true },
    { "label": "Fase 3", "done": false }
  ]
}
```

### Valores válidos para `status`

| Valor | Badge visible | Significado |
|-------|---------------|-------------|
| `dev` | "En Desarrollo" | Proyecto activo en construcción |
| `active` | "Producción" | Ya en uso / terminado |
| `planning` | "Planificación" | Aún en fase de discovery |
| `paused` | "Pausado" | Detenido temporalmente |

### 2. Agregar imágenes

Pon las capturas en `images/` y referéncialas en `projects.json`:

```json
"images": [
  { "src": "images/mi-proyecto-home.png", "caption": "Pantalla principal" },
  { "src": "images/mi-proyecto-login.png", "caption": "Login" }
]
```

Recomendaciones para las imágenes:
- Formato: PNG o JPG
- Aspect ratio: 16:10 (ej: 1600x1000px)
- Tamaño: < 500 KB por imagen (optimizar con TinyPNG o similar)
- **Sin datos sensibles**: revisa bien antes de subir. Si hay nombres reales, datos de clientes, URLs internas — ofúscalos.

### 3. Verificar localmente

```bash
# Desde la raíz del proyecto
python3 -m http.server 8000
# Abrir http://localhost:8000
```

O con Node:

```bash
npx serve .
```

---

## Despliegue en GitHub Pages

1. Crea un repo público en tu GitHub personal (ej: `portafolio`)
2. Sube los archivos:
   ```bash
   git init
   git add .
   git commit -m "Portafolio inicial"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/portafolio.git
   git push -u origin main
   ```
3. En el repo → **Settings → Pages → Source: `main` / root** → Save
4. Espera ~1 minuto. Tu portafolio estará en `https://TU_USUARIO.github.io/portafolio/`

⚠️ **Antes de subir:** revisa cada proyecto. Si hay info confidencial del trabajo (nombres de clientes, sistemas internos, URLs privadas, capturas con datos reales), sustitúyelos por nombres genéricos o captura data ofuscada. El repo va a ser público.

---

## Flujo recomendado: generar datos con Claude Code

En lugar de escribir el `projects.json` a mano, puedes usar **Claude Code** dentro de cada uno de tus proyectos para que analice el repo y genere un archivo `.portfolio.md` con toda la info lista para copiar al JSON.

### Prompt para Claude Code

Ejecuta este prompt **dentro de cada proyecto** (desde la raíz del repo del proyecto, no desde el portafolio):

````
Analiza a fondo este proyecto y genera un archivo llamado `.portfolio.md` en la raíz del repositorio con la estructura que te indico al final.

INSTRUCCIONES:
1. Revisa la estructura de carpetas y los archivos de dependencias (package.json, requirements.txt, pom.xml, Cargo.toml, go.mod, etc.) para identificar el stack técnico real.
2. Revisa el README existente (si hay) y el log de git (últimos 30 commits) para extraer contexto de qué hace el proyecto y en qué se ha trabajado.
3. Rellena TODOS los campos que puedas inferir del código. Lo que no puedas inferir con certeza, déjalo como null o con "REVISAR: ..." para que yo lo complete.
4. NO INVENTES datos. Si no conoces el rol, prioridad, cliente, o fechas, usa placeholder.
5. Para `progress` (0-100), estima basándote en: cantidad de TODOs/FIXMEs, features pendientes en README, presencia de tests, madurez del código, y lo que veas en commits recientes. Si no puedes estimar, pon null.
6. Para `status`, usa uno de: "dev" (en desarrollo), "active" (producción/terminado), "planning" (discovery), "paused" (pausado).
7. Para los hitos (`milestones`), extrae 4-6 fases del proyecto basándote en la estructura de features. Marca como `"done": true` las que veas completadas, `"wip": true` la que esté en progreso ahora.
8. Para `current` y `missing`, usa los commits recientes y TODOs del código.
9. Agrupa el stack en: frontend, backend, database, infra, otros. Si una categoría no aplica, deja array vacío.
10. Para `images` deja un array vacío — yo las agregaré manualmente.
11. Al final, muéstrame un resumen de qué campos necesito revisar.

ESTRUCTURA EXACTA del archivo a generar (formato JSON dentro del markdown para facilitar el copy-paste al projects.json final):

```markdown
# [Nombre del proyecto]

## Datos para el portafolio

```json
{
  "id": null,
  "slug": "kebab-case-del-nombre",
  "title": "Nombre legible",
  "subtitle": "Categoría · Subcategoría",
  "status": "dev",
  "statusLabel": "En Desarrollo",
  "progress": null,
  "eta": "REVISAR",
  "priority": "REVISAR",
  "type": "",
  "role": "REVISAR",
  "team": "REVISAR",
  "client": "REVISAR",
  "startDate": null,
  "estimatedEnd": "REVISAR",
  "desc": "",
  "current": "",
  "missing": "",
  "stack": {
    "frontend": [],
    "backend": [],
    "database": [],
    "infra": [],
    "otros": []
  },
  "images": [],
  "milestones": [
    { "label": "", "done": false }
  ]
}
```

## Resumen de lo que necesito revisar

- [ ] Campo X: ...
- [ ] Campo Y: ...
```
````

### Flujo completo paso a paso

1. En cada proyecto, abres Claude Code y pegas el prompt
2. Claude Code genera `.portfolio.md` con lo que puede inferir
3. Abres el archivo, revisas los `REVISAR` y los completas
4. Copias el bloque JSON al array `projects` de `data/projects.json` del portafolio
5. Asignas un `id` único (incrementando desde el último)
6. Agregas las capturas de pantalla en `images/`
7. Pruebas localmente y haces `git push` → GitHub Pages actualiza solo

---

## Personalización del look

Colores principales en el CSS (busca `:root` en el `<style>`):

```css
--gold: #c9a84c;       /* acento principal */
--gold-light: #e8c97a;
--bg: #080808;         /* fondo */
--text: #f0ede6;       /* texto */
--muted: #6b6560;      /* texto secundario */
```

Cambia estos 5 valores y tienes otro tema (ej. turquesa + navy, rojo + crema, etc.).

### Cambiar fuentes

En el `<head>` hay un `<link>` de Google Fonts. Actualmente:
- **Bebas Neue** — títulos
- **DM Mono** — cuerpo y UI
- **Playfair Display** — importada pero no usada (disponible si la quieres)

---

## Checklist antes de publicar

- [ ] Revisé que no haya info confidencial en `projects.json`
- [ ] Capturas ofuscadas / sin datos reales sensibles
- [ ] Nombres de clientes internos genéricos si corresponde
- [ ] Probé localmente y todo carga
- [ ] README actualizado con mis datos
- [ ] Validé con mi jefe/equipo que puedo publicar esto

---

## Troubleshooting

**El portafolio muestra "ERROR AL CARGAR"**
- Probablemente abriste `index.html` directamente (file://) y el `fetch` del JSON falla por CORS. Levanta un servidor local (`python3 -m http.server`) o súbelo a GitHub Pages.

**Las imágenes no aparecen**
- Revisa que la ruta en `"src"` coincida con el archivo real. Es sensible a mayúsculas/minúsculas.

**La intro tarda mucho**
- Después de la primera vez se guarda en `sessionStorage` y no vuelve a salir. Para forzarla, abre en ventana incógnito o borra el storage.

**Las barras de progreso no animan**
- Usa el `IntersectionObserver`. Si el navegador es muy viejo, no anima pero el valor se ve igual.

---

## Mantenimiento con Claude Code — Flujo establecido

Este portafolio se mantiene con ayuda de **Claude Code** desde `C:\desarrollos\portafolio`.  
Los prompts listos para usar están en `PROMPT_NUEVO_PROYECTO.md`.

### Archivos clave

| Archivo | Para qué sirve |
|---------|---------------|
| `data/projects.json` | Fuente de verdad — el portafolio lee de aquí |
| `*.portfolio.md` | Uno por proyecto, respaldo legible y detallado |
| `PROMPT_NUEVO_PROYECTO.md` | Prompts listos para Claude Code |

### Agregar un proyecto nuevo

1. Asegúrate de que la carpeta del proyecto esté en `C:\desarrollos\`
2. Abre Claude Code en `C:\desarrollos\portafolio`
3. Pega el **Prompt Paso 1** de `PROMPT_NUEVO_PROYECTO.md` con el nombre de la carpeta
4. Claude explorará el proyecto, generará el `.portfolio.md` y lo agregará a `data/projects.json`
5. Completa los campos `REVISAR` de forma interactiva con Claude

### Actualizar avances de un proyecto

Dile a Claude algo como:

```
Actualiza el proyecto `[slug]` en data/projects.json:
- progress: [número]
- eta: "[tiempo restante]"
- current: "[qué se está haciendo ahora]"
- missing: "[qué falta]"
- Marca el milestone "[texto del hito]" como done: true
Actualiza meta.lastUpdate a hoy y valida el JSON.
```

### Cambiar status de un proyecto

```
Cambia el proyecto `[slug]` en data/projects.json:
- status: "active" / "paused" / "dev"
- statusLabel: "Producción" / "Pausado" / "En Desarrollo"
- eta: "En vivo" / "En espera" / "[tiempo]"
- progress: [número]
Actualiza meta.lastUpdate a hoy y valida el JSON.
```

### Mapeo de status

| En `.portfolio.md` | En `projects.json` | Label visible |
|--------------------|--------------------|---------------|
| `en_proceso` | `dev` | En Desarrollo |
| `terminado` | `active` | Producción |
| `pausado` | `paused` | Pausado |
| `planeado` | `planning` | Planificación |

---

## Licencia

Personal. Úsalo, modifícalo, compártelo.
