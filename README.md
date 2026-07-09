# Propuesta Modular SCM — Sincronización real con Firebase

## Ya está todo el código listo. Solo falta UN paso tuyo: crear el proyecto
## de Firebase y pegar 6 datos en el archivo. 5 a 10 minutos, gratis.

No inventé ninguna cuenta ni ninguna clave — eso solo lo podés crear vos
(requiere aceptar los términos de Google con tu propia cuenta). Una vez
que me pases —o pegues vos mismo— esos 6 datos, la sincronización en
tiempo real entre celular y notebook queda funcionando de punta a punta.

---

## Paso 1 — Crear el proyecto de Firebase

1. Entrá a **https://console.firebase.google.com** con tu cuenta de Google.
2. "Agregar proyecto" → ponele un nombre, por ejemplo `propuesta-scm`.
3. Podés desactivar Google Analytics en este proyecto (no hace falta).
4. Esperá a que termine de crearse (unos 30 segundos).

## Paso 2 — Activar Firestore

1. En el menú de la izquierda: **Compilación → Firestore Database**.
2. "Crear base de datos".
3. Elegí **"Iniciar en modo de producción"**.
4. Elegí la ubicación del servidor (cualquiera de Sudamérica, ej.
   `southamerica-east1`, va a andar bien).

## Paso 3 — Configurar las reglas de acceso de Firestore

Dentro de Firestore Database → pestaña **"Reglas"**, reemplazar todo el
contenido por esto y publicar:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /propuestas/{docId} {
      allow read, write: if true;
    }
  }
}
```

**Importante — leer esto:** esta regla es intencionalmente abierta, porque
la app no tiene un sistema de login. Cualquiera que sepa (o adivine) el
"código de propuesta" exacto que le pongas a un cliente va a poder leerlo
y editarlo. Por eso:
- Usá códigos largos y no obvios (ej. `grido-cba-8f2k91`, no `cliente1`).
- No pongas ahí información ultra sensible (montos finales de contrato,
  datos personales) — es una propuesta comercial editable, no una caja
  fuerte.

## Paso 4 — Registrar una app Web y copiar la configuración

1. En la página principal del proyecto (ícono de engranaje → "Configuración
   del proyecto"), bajar hasta "Tus apps".
2. Tocar el ícono **</>** (Web).
3. Ponerle un apodo (ej. "propuesta-web") y "Registrar app". **No hace
   falta** activar Firebase Hosting.
4. Firebase te muestra un bloque de código con un objeto `firebaseConfig`
   parecido a esto:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "propuesta-scm.firebaseapp.com",
  projectId: "propuesta-scm",
  storageBucket: "propuesta-scm.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

## Paso 5 — Pegar esos datos en `index.html`

Abrí `index.html` con cualquier editor de texto (o directo en GitHub, con
el lápiz de "editar archivo") y buscá este bloque, cerca del final:

```js
const firebaseConfig = {
    apiKey: "PEGAR_AQUI",
    authDomain: "PEGAR_AQUI",
    projectId: "PEGAR_AQUI",
    storageBucket: "PEGAR_AQUI",
    messagingSenderId: "PEGAR_AQUI",
    appId: "PEGAR_AQUI"
};
```

Reemplazá cada `"PEGAR_AQUI"` por el valor real que te dio Firebase en el
Paso 4. Guardar, subir a GitHub pisando el `index.html` anterior.

Si preferís, pegame acá los 6 valores y yo te devuelvo el archivo ya
completado — como quieras.

---

## Cómo se usa, una vez configurado

- Arriba de los botones hay un campo **"Código de esta propuesta"**.
  Ese código es la clave: si abrís el sitio en el celular y en la
  notebook con el **mismo código**, van a ver y editar exactamente el
  mismo contenido, en tiempo real (con conexión a internet).
- El indicador de estado te dice en qué modo está:
  - 🟢 Sincronizado en la nube — cambios visibles en todos los dispositivos
    que usen el mismo código.
  - 🟡 Nube no configurada — solo se guarda en este dispositivo (pasa esto
    hasta que completes los Pasos 1 a 5).
  - 🔴 Error de conexión — revisar las reglas de Firestore (Paso 3) o la
    conexión a internet.
- Si estás sin señal, la app sigue funcionando y guardando localmente;
  apenas vuelva la conexión, sincroniza sola.
- El botón **"Descargar Archivo Personalizado"** sigue estando disponible
  para cuando quieras un archivo `.html` final y cerrado para mandarle al
  cliente por mail, sin depender de que abra ningún link en vivo.

---

## Qué se corrigió en esta versión (resumen de cambios previos)

1. **Responsive real**: el archivo original solo tenía estilos para
   impresión. Se agregó una media query para celular que apila las tablas,
   botones y sliders en una sola columna.
2. **PWA**: manifest + ícono + service worker, para poder "instalar" el
   sitio en el celular como si fuera una app.
3. **Service worker corregido**: ahora prioriza siempre la versión más
   nueva del sitio (network-first) en vez de quedarse con una copia vieja
   en caché.
4. **No se perdió contenido**: se verificó línea por línea contra tu
   archivo original — nada de tu texto fue borrado o modificado, solo se
   agregaron estas capas nuevas.

---

## Publicación

Repo: https://github.com/gustavonardone1965-cmyk/Consultnet/
Sitio: https://gustavonardone1965-cmyk.github.io/Consultnet/

Subir los 5 archivos (`index.html`, `manifest.json`, `sw.js`,
`icon-192.png`, `icon-512.png`) pisando los existentes.
