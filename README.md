# APPS Retro

Escritorio de los años 80 en el navegador. Al hacer clic en el computador, se enciende, muestra el logo SM, arranca **SM-DOS** y la cámara se acerca al monitor. Desde la consola se pueden ejecutar los juegos guardados en `C:\JUEGOS`.

No necesita instalación ni servidor: basta con abrir `index.html` en el navegador.

## Cómo se usa

1. Abre `index.html`.
2. Haz clic en el computador.
3. Espera a que arranque SM-DOS. Al terminar, la consola muestra el contenido de `C:\JUEGOS`.
4. Escribe el nombre de un juego y pulsa Enter.

### Comandos de SM-DOS

| Comando | Qué hace |
|---|---|
| `DIR` | Lista los archivos del directorio actual |
| `CD JUEGOS`, `CD..`, `CD\` | Cambia de directorio |
| `CLS` | Borra la pantalla |
| `VER` | Muestra la versión |
| `AYUDA` o `HELP` | Lista los comandos |
| `INVASION`, `BLOQUES`, `RATON` | Ejecuta el juego (desde `C:\JUEGOS`) |

## Juegos

Todos se ejecutan dentro del monitor. `ESC` vuelve a SM-DOS, `P` pausa y el récord de cada juego se guarda en el navegador.

| Juego | Descripción | Controles |
|---|---|---|
| **INVASION.EXE** | Una flota alienígena baja en formación; destrúyela antes de que llegue al suelo. | `←` `→` mover · `ESPACIO` disparar |
| **BLOQUES.EXE** | Encaja las piezas que caen y completa líneas para borrarlas. | `←` `→` mover · `↑`/`X` girar · `Z` girar al revés · `↓` bajar · `ESPACIO` soltar |
| **RATON.EXE** | Un ratón come queso en un laberinto mientras cuatro gatos lo persiguen. El queso grande hace que los gatos huyan. | Flechas o `WASD` |

## Estructura

```
index.html                  Página principal
assets/
  css/styles.css            Escena, pantalla CRT, consola y zoom
  img/fondo0.png            Imagen del escritorio (la única que se usa)
  js/
    app.js                  Encendido, arranque de SM-DOS, zoom y comandos
    juegos.js               Lista de archivos de C:\JUEGOS
    juegos/
      invasion.js
      bloques.js
      raton.js
```

El encendido, el logo y la consola se dibujan con código sobre `fondo0.png`. Las demás imágenes de `assets/img/` (`fondo.png`, `fondo1.png` y los logos) no se usan; se guardaron como referencia.

## Añadir un juego

1. Crea `assets/js/juegos/mijuego.js` con un objeto global que tenga el método `iniciar(contenedor, alSalir)`:

   ```js
   window.MiJuego = (() => {
       function iniciar(contenedor, alSalir) {
           const canvas = document.createElement("canvas");
           canvas.width = 320;
           canvas.height = 240;
           contenedor.append(canvas);
           // ... bucle del juego y teclado ...
           // Al pulsar ESC: quitar listeners, canvas.remove() y llamar a alSalir()
       }
       return { iniciar };
   })();
   ```

   El canvas de 320×240 se escala para ocupar la pantalla del monitor. Puedes copiar cualquiera de los juegos existentes como plantilla.

2. Cárgalo en `index.html` antes de `juegos.js`:

   ```html
   <script src="assets/js/juegos/mijuego.js"></script>
   ```

3. Añádelo a la lista de `assets/js/juegos.js`. El nombre puede tener hasta 8 caracteres:

   ```js
   { nombre: "MIJUEGO", ext: "EXE", tam: 20480, fecha: "10-11-86", programa: "MiJuego" },
   ```

   En lugar de `programa` puedes poner `url: "ruta/a/pagina.html"` para abrir el juego en otra página.

## Ajustes

- **Tiempos de la introducción:** las constantes al principio de `assets/js/app.js`. Si cambias `DURACION_ZOOM`, cambia también la `transition` de `.escena` en el CSS.
- **Posición de la pantalla sobre la imagen:** `.pantalla` (`left`, `top`, `width` y `height`) en `styles.css`.
- **Zona en la que se puede hacer clic:** `.computador` en `styles.css`.

## Limitaciones

- Para escribir en SM-DOS y jugar hace falta un teclado físico; en móviles y tabletas no aparece el teclado en pantalla.
