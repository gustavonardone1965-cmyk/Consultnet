# Propuesta Modular SCM — Cómo publicarla

Esta carpeta contiene una PWA (Progressive Web App). Una vez publicada en un
link https://, el cliente la abre desde el celular y le queda instalada como
una app: ícono propio, pantalla completa, funciona offline.

Archivos incluidos:
- `index.html` — la propuesta
- `manifest.json` — nombre, ícono y modo de apertura de la app
- `sw.js` — service worker (cachea todo para que funcione sin internet)
- `icon-192.png` / `icon-512.png` — ícono de la app

No modificar los nombres de estos archivos: `index.html` los referencia
por nombre exacto.

---

## Opción A — GitHub Pages (gratis y permanente)

**1. Crear la cuenta**
Entrá a github.com → "Sign up" → completá email, usuario y contraseña.

**2. Crear el repositorio**
- Ícono "+" (arriba a la derecha) → "New repository"
- Nombre, por ejemplo: `propuesta-scm`
- Dejalo en **Public** (Pages gratis solo funciona con repos públicos)
- No marcar "Add a README"
- "Create repository"

**3. Subir los archivos**
- En la página del repo, tocá "uploading an existing file"
- Arrastrá los 5 archivos de esta carpeta **sueltos** (no el zip, no la carpeta)
- Abajo, "Commit changes"

**4. Activar Pages**
- Pestaña **Settings** → menú izquierdo **Pages**
- "Branch": elegir `main` y carpeta `/ (root)` → **Save**
- Esperar 1-2 minutos

**5. Si aparece "Upgrade or make this repository public to enable Pages"**
El repo quedó privado. Para arreglarlo:
- **Settings** → bajar hasta la zona roja "Danger Zone"
- "Change repository visibility" → "Change to public"
- Escribir el nombre del repo para confirmar
- Volver a **Settings → Pages** y activar como en el paso 4

Tener en cuenta: un repo público significa que cualquiera con el link
exacto puede ver el contenido. Sirve para la plantilla en blanco; si más
adelante volcás ahí datos confidenciales de un cliente puntual, usar la
Opción B en su lugar.

**6. Conseguir el link**
Refrescar Settings → Pages. Aparece en verde:
`Your site is live at https://tu-usuario.github.io/propuesta-scm/`
Ese es el link para el cliente.

**7. Actualizar contenido más adelante**
"Add file" → "Upload files" en el repo, subir la versión nueva de
`index.html` pisando la anterior. Se actualiza solo en un par de minutos.

---

## Opción B — Netlify Drop (gratis, sin cuenta, y podés mantenerlo no listado)

1. Entrar a **netlify.com/drop**
2. Arrastrar esta carpeta completa (`app`) al navegador
3. En segundos entrega una URL tipo `https://nombre-random.netlify.app`
4. Ese es el link para el cliente

Ventaja frente a GitHub Pages: no requiere que el código sea público en
ningún repositorio, y no hace falta cuenta para el primer despliegue.
Si querés poder actualizar el mismo sitio después (en vez de generar uno
nuevo cada vez), conviene crear una cuenta gratuita en Netlify antes de
arrastrar la carpeta.

---

## Instalar la app en el celular (una vez publicada)

1. Abrir el link publicado desde **Chrome en Android**
2. Va a aparecer un aviso "Instalar app" (o desde el menú ⋮ → "Instalar
   aplicación" / "Añadir a pantalla de inicio")
3. Confirmar. Queda el ícono en la pantalla de inicio del celular, se abre
   a pantalla completa y funciona sin conexión.
