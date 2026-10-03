# contra-evolution-arcade-2player-fix
Configurador Win32 y proxy dinput8.dll para habilitar soporte síncrono de 2 jugadores en Contra Evolution, eliminar micro-tirones (stuttering) y liberar el teclado compartido.

# Contra Evolution - Arcade 2-Player & Anti-Stuttering Fix
Para este desarrollo se uso la base de [geeky/dinput8wrapper](https://github.com/geeky/dinput8wrapper), se mejoro y fue compilado por **Emanuel42** para Contra Evolution.  
Suite portable nativa en C++ para cabinas arcade y muebles recreativos.

---

## 🕹️ Descripción del Proyecto
Este proyecto provee la solución definitiva para operar **Contra Evolution** de forma comercial y cooperativa en sistemas arcade de PC. Resuelve de raíz los dos problemas más graves del motor original del juego: el bloqueo del teclado compartido y los micro-congelamientos de pantalla (*stuttering*) causados por el Kernel de Windows al escanear los puertos USB.

Este fix incluye dos binarios portables sin dependencias externas:
1. **`Config_Joystick.exe`:** Configurador visual interactivo con soporte de carga automática (*Auto-Load*) y auto-cierre para calibrar las palancas cómodamente.
2. **`dinput8.dll`:** Inyector proxy de bajo nivel que intercepta las señales HID (*Raw Input*), congela el radar del Kernel de Windows eliminando los tirones y procesa los candados de hardware para el Player 1 y Player 2 en paralelo.

---

## 🛡️ Soporte Nativo de Xbox y Configuración por Defecto
La `dinput8.dll` incorpora un **Escudo de Contingencia de Fábrica**. Esto significa que si no existe un archivo de configuración previo en la carpeta, el sistema **reconoce automáticamente los mandos de Xbox 360 / Xbox One y Joysticks Genéricos USB**, aplicando de forma inmediata el siguiente mapa de botones nativo:

### 🎮 Mapa por Defecto (Default Mode)
* **Movimiento (P1 y P2):** Ejes Analógicos tradicionales del Stick Izquierdo (Arriba, Abajo, Izquierda, Derecha).
* **Botonera Física Recreativa:**
  * **Disparo:** Botón 3 (X en Xbox) ➡️ Emula la tecla `J` (P1) y `Numpad 1` (P2).
  * **Salto:** Botón 1 (A en Xbox) ➡️ Emula la tecla `K` (P1) y `Numpad 2` (P2).
  * **Cambio de Arma:** Botón 4 (Y en Xbox) ➡️ Emula la tecla `U` (P1) y `Numpad 3` (P2).
  * **Coin (Fichas):** Botón 5 (LB en Xbox) ➡️ Emula la tecla `Espacio` en ambos jugadores.
  * **Pausa:** Botón 7 (Back / View en Xbox) ➡️ Emula la tecla `P` en ambos jugadores.
  * **Start:** Botón 8 (Start / Menu en Xbox) ➡️ Emula la tecla `Enter` (P1) e `Intro Numpad` (P2).
  * **Cierre de Juego (Exit):** Botón 10 (Click Stick Derecho en Xbox) ➡️ Emula la tecla `Esc` (Solo Player 1).

👉 **NOTA DE USO:** Si esta distribución de fábrica no se adapta a la botonera de tu mueble o controladora USB, simplemente abres **`Config_Joystick.exe`**, presionas las casillas para remapear los botones físicos a tu gusto y guardas la configuración.

---

## 🔧 Instrucciones de Instalación y Calibración
1. Descarga el paquete compilado desde la sección de **Releases**.
2. Copia los archivos `Config_Joystick.exe` y `dinput8.dll` y pégalos directamente en la carpeta raíz del juego (donde está el archivo ejecutable principal `.exe`).
3. **Mapeo Personalizado (Opcional):** Ejecuta `Config_Joystick.exe`. Haz clic en las casillas para registrar las palancas y botones de tu tablero arcade. Al finalizar, haz clic en el botón gigante **"ACEPTAR Y GUARDAR CONFIGURACION"**. La herramienta creará el archivo plano de datos `dinput8.ini` de forma automática y la suite se cerrará sola.
4. Inicia el juego. El parche inyectará los controles al vuelo de forma comercial impecable.

---

## 📂 Formato de Datos Generado (`dinput8.ini`)
El configurador guardará tus parámetros de forma nativa en texto plano, dividiendo el hardware para que el mantenimiento del mueble sea sumamente directo:

```ini
[Joystick1_Player1]
Modo_Arriba = 1                                // 1 = Eje Analógico | 2 = POV Hat | 3 = Botón físico
Valor_Arriba = 1                               // ID real del botón o eje en la controladora USB
Disparo = 3                                    // ID físico del botón soldado en la placa
```

---

## 🔀 Combinación de Parches: ¡Controles + Pantalla Completa 16:9!
Este proyecto fue diseñado con arquitectura elástica portable, lo que significa que **se puede combinar perfectamente** con soluciones de escalado de video para lograr la experiencia arcade definitiva en monitores modernos de 1080p, 2K o 4K sin pérdida de FPS.

Si deseas forzar el juego a pantalla completa sin bordes y estirar la imagen a 16:9, puedes acoplar este fix de controles junto con el wrapper de Direct3D 9, utilizando cualquiera de estas dos opciones:
* El proyecto original de ThirteenAG: [ThirteenAG/d3d9-wrapper](https://github.com)
* Mi compilación manual optimizada: [Emanuel42-Soucre/d3d9-wrapper-custom-resolution](https://github.com)

### 🚀 Cómo combinarlos en tu carpeta (Instrucciones de Mueble Arcade):
1. Sigue los pasos de instalación de este fix de controles depositando la `dinput8.dll` y el configurador `Config_Joystick.exe`.
2. Descarga el wrapper de video y copia los archivos **`d3d9.dll`** and **`d3d9.ini`** dentro de la misma carpeta raíz del juego.
3. Modifica la resolución deseada en el fichero `d3d9.ini` (ejemplo: `Width = 1920`, `Height = 1080`).
4. **🔥 REGLA DE ORO DE VIDEO (¡IMPORTANTE!):** Para jugar en modo panorámico real (16:9), debes iniciar el juego ejecutando estrictamente el archivo **`AMContra.exe`** que se encuentra dentro de la subcarpeta **`Windowed Mode`** (se recomienda preferentemente utilizar la versión de máxima calidad de **`1600x1200`**). Esto le entrega al inyector D3D9 una imagen limpia de alta densidad para estirarla de forma simétrica a toda tu pantalla.
5. **📺 NOTA PARA ENTUSIASTAS RETRO (4:3 Puro):** No utilices el ejecutable de la carpeta *Windowed Mode* si planeas usar el ejecutable original de la carpeta *Full Screen Mode*. El juego se forzará de forma rígida a la relación de aspecto antigua de 4:3. En ese caso específico, **NO debes copiar los archivos `d3d9.dll` y `d3d9.ini`** en el directorio, ya que la `dinput8.dll` unificada de Emanuel42 se encargará de gestionar las palancas de forma fluida e independiente por sí sola en segundo plano.
