# Proyecto de gráfica interactivo

Aplicación C++ con OpenGL 3.3 para visualizar el escenario deportivo exportado desde Blender. El proyecto incluye los archivos fuente, el modelo GLB, shaders y documentación.

## Contenido

- `assets/escenario.glb`: escena exportada con materiales, texturas, personajes y clips de animación del archivo Blender.
- `src/main.cpp`: importación de la escena con Assimp, animación por huesos, cámaras, iluminación, controles y marcador.
- La formación se completa con 22 instancias del rig de jugador ya incluido en el GLB; no requiere descargar modelos nuevos.
- `resources/shaders`: shaders OpenGL 3.3 para materiales, iluminación y HUD.
- `vcpkg.json`: dependencias reproducibles.
- `tools/ajustar_escena.py`: corrección documentada de escala y colocación para la escena GLB original. Se ejecuta una sola vez sobre la exportación sin corregir.

La exportación original contiene 452 nodos, 387 mallas, 93 materiales, 9 imágenes, dos esqueletos y 19 clips. Tras reconstruir las porterías y las torres, la escena ajustada contiene 563 nodos. Se conservan el jugador, el árbitro, los cuatro banderines y los clips `Jugador CHICHARITO Carrera.004`, `Balon_GolAction.005` y `Arbitro Señal de falta.004`.

La secuencia ahora inicia automáticamente con una toma aérea del autobús llegando al estadio, pasa a una vista general de los dos equipos (11 contra 11) y el árbitro en el círculo central, muestra la carrera y el balón entrando a la portería, suma un gol al marcador y termina con el vehículo de emergencias recogiendo a un jugador lesionado. La escena también muestra el carrito de alimentos, banderines, dos porterías, luminarias, marcador, mascota, trofeo, podio, bancas, gradas y accesos. El árbitro se coloca en el centro; el vehículo de emergencias está reducido y tiene una ruta de entrada y salida. La integración de varios jugadores reutiliza el rig cargado del GLB y su animación de huesos.

## Compilar en Windows con Visual Studio 2022

Instala Visual Studio 2022 con la carga de trabajo **Desarrollo para el escritorio con C++** (incluye herramientas x86/x64), CMake y Git. Instala vcpkg y define `VCPKG_ROOT` en PowerShell:

```powershell
$env:VCPKG_ROOT = "C:\vcpkg"
& "$env:VCPKG_ROOT\bootstrap-vcpkg.bat"
```

Desde esta carpeta del proyecto:

```powershell
cmake --preset windows-win32
cmake --build build/win32 --config Release
.\build\win32\Release\proyectoGrafica_OpenGL.exe
```

El preset `windows-win32` configura Visual Studio 2022 para compilar Win32 (x86) con el triplet `x86-windows-static`. La primera configuración descarga Assimp, GLEW, GLFW, GLM y stb con vcpkg; se requiere internet. Abre la carpeta que contiene `CMakeLists.txt` en Visual Studio, selecciona el preset `windows-win32` y compila `Release`.

## Controles

| Tecla/entrada | Acción |
| --- | --- |
| `Espacio` | Reproducir o pausar la secuencia completa |
| `R` | Regresar al inicio y pausar |
| `G` | Ir a la jugada de gol y reproducir desde ahí |
| `F` | Ir a la señal del árbitro y reproducir desde ahí |
| `1` / `2` | Cámara en primera persona / cámara de transmisión |
| `3` | Cámara cinematográfica con tomas automáticas de bus, partido y emergencia |
| `W A S D` | Caminar en primera persona; en transmisión, orbitar y acercar/alejar |
| Ratón | Girar la cámara; rueda para cambiar el campo de visión |
| `L` | Encender o apagar las luces del estadio |
| `N` | Cambiar entre día y noche |
| `J` / `K` | Sumar un gol al equipo local / visitante |
| `Retroceso` | Reiniciar el marcador |
| `Esc` | Cerrar |

## Relación con los requisitos de la consigna

| Requisito | Implementación / evidencia prevista |
| --- | --- |
| OpenGL moderno | Contexto OpenGL 3.3 Core y shaders GLSL 330 |
| Escenario deportivo y zonas | GLB con cancha y marcaciones, gradas en nivel inferior y superior, además de accesos/plaza exterior. |
| Personajes y modelado jerárquico | Los rigs del jugador y del árbitro se conservan en el GLB; Assimp importa los pesos, huesos y jerarquías. |
| Carga de modelos | Importador de GLB en tiempo de ejecución mediante Assimp. |
| Perspectiva, materiales y texturas | Proyección perspectiva; mapas embebidos del GLB; shader de iluminación difusa y mapa de sombras direccionales PCF. |
| Iluminación y día/noche | Luz ambiental/direccional y cuatro focos conmutables en modo noche (`N`, `L`). Mapa de sombras direccionales PCF actualizado cada fotograma; `N` y `L` controlan el ciclo día/noche y los focos. |
| Cámara en primera y tercera persona | `1` activa cámara caminable y ratón; `2` activa cámara de transmisión orbital. |
| Animación por transformaciones y huesos | Clips del GLB para carrera, balón, señal y bus; la secuencia reproduce el gol y actualiza el marcador, y anima la llegada/salida del vehículo médico y el traslado del jugador lesionado. |
| Interacción | Teclado, ratón, luces y marcador local. `G` y `F` permiten mostrar directamente las secuencias principales. |
| Sonido (opcional) | No incluido. |
| Memoria y Acta | Plantillas en `DOCUMENTACION_TECNICA.md` y `ACTA.md`; completar autores, capturas, video, explicación de zonas y resultados al ejecutar en Windows. |

## Estado de verificación

La entrega está configurada para Win32 (x86), que es la plataforma requerida para tu prueba. Al iniciar, el programa crea `startup.log` en la carpeta del ejecutable; si se cierra, ese archivo indica la última etapa alcanzada y el código de excepción. La compilación local en este entorno no fue posible porque CMake y GLEW no están instalados; el flujo de Windows compila un paquete Win32. Revisa la formación, las tomas, el gol/marcador y el traslado médico en la VM. Los campos de integrantes, firmas, capturas comparativas y video de demostración siguen requiriendo información y evidencia real del equipo.
