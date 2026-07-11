# Propuesta Modular SCM — Fix de edición de texto y Undo

## Qué se rompió y por qué

**Síntoma:** al escribir en un campo editable, el cursor "volvía para atrás"
y el texto salía invertido.

**Causa real:** la sincronización en tiempo real con Firestore recibía de
vuelta tu propio cambio cada ~700ms y volvía a "pintar" el `innerHTML`
completo de todos los campos editables — incluyendo el que tenías activo
en ese momento, mientras seguías escribiendo ahí. Cada repintado reiniciaba
el cursor a la posición 0, así que la próxima tecla se insertaba antes de
lo ya escrito. Es un bug clásico de sincronización en tiempo real cuando no
se protege el campo con foco activo.

**Además:** el botón "Volver Atrás (Undo)" solo guardaba el estado de los
checkboxes y sliders de los módulos — nunca el texto. Por eso, aunque
tildaras y destildaras módulos el undo "funcionaba", pero para el texto
nunca había nada que deshacer.

## Qué se corrigió

1. **`applyState()` ya no pisa el campo que tenés enfocado.** Antes de
   sobrescribir un `contenteditable`, un checkbox o un slider, revisa si
   ese elemento es el que tiene el foco (`document.activeElement`) — si lo
   es, lo deja en paz. Esto resuelve el cursor invertido de raíz.
2. **Se ignora el "eco" de Firestore.** Cuando escribís algo, Firestore te
   devuelve dos avisos: uno inmediato (todavía no confirmado por el
   servidor) y uno confirmado. Ahora se ignora el primero
   (`hasPendingWrites`), porque el DOM ya tiene ese contenido — no hace
   falta repintarlo, y repintarlo era justamente lo que generaba el
   problema.
3. **El Undo ahora cubre TODO: texto, checkboxes y sliders.** Se unificó
   con el mismo sistema que ya se usaba para el borrador local. Cada vez
   que escribís, tildás algo o movés un slider, un momento después (medio
   segundo de pausa) queda guardado un "punto de retroceso". "Volver Atrás"
   ahora restaura texto incluido, no solo módulos.
4. **El botón Undo se habilita solo.** Empieza deshabilitado (no hay nada
   que deshacer todavía) y se habilita automáticamente en cuanto hacés el
   primer cambio real después de cargar la página.

## Cómo probarlo

1. Escribí varias oraciones seguidas en cualquier campo editable — el
   cursor ya no debería saltar ni invertir el texto.
2. Hacé 2 o 3 cambios distintos (texto, un módulo, un slider).
3. El botón "Volver Atrás (Undo)" debería estar habilitado — tocalo y
   confirmá que vuelve al estado anterior, texto incluido.
4. Si tenés dos pestañas abiertas con el mismo código de propuesta,
   escribí en una: la otra se actualiza sola, y la que estás usando para
   escribir no se ve afectada mientras estás tipeando en ella.

No se perdió ningún contenido del archivo original — se verificó línea por
línea contra tu archivo subido; todos los cambios caen exclusivamente
dentro del bloque `<script>` de lógica, nada del texto de las secciones 0
a 7 fue tocado.

---

## Publicación

Repo: https://github.com/gustavonardone1965-cmyk/Consultnet/
Sitio: https://gustavonardone1965-cmyk.github.io/Consultnet/

Subir `index.html` pisando el existente. `manifest.json`, `sw.js` e íconos
no cambiaron en esta corrección — no hace falta volver a subirlos si ya
están en el repo.
