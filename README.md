# Proyecto de gráfica interactivo

Aplicación C++ con OpenGL 3.3 para visualizar el escenario deportivo exportado desde Blender. Este directorio está fuera del repositorio `proyectoGrafica`; los cambios aquí no se han subido ni mezclado con el repo.

## Contenido

- `assets/escenario.glb`: escena exportada con materiales, texturas, personajes y clips de animación del archivo Blender.
- `src/main.cpp`: importación de la escena con Assimp, animación por huesos, cámaras, iluminación, controles y marcador.
- `resources/shaders`: shaders OpenGL 3.3 para materiales, iluminación y HUD.
- `vcpkg.json`: dependencias reproducibles.

La exportación contiene 452 nodos, 387 mallas, 93 materiales, 9 imágenes, dos esqueletos y 19 clips. Se verificaron en el GLB el jugador, el árbitro, el balón de gol, la portería, cuatro banderines y los objetos animados de autobús y carrito. Las animaciones clave exportadas son `Jugador CHICHARITO Carrera.004`, `Balon_GolAction.005` y `Arbitro Señal de falta.004`.

## Compilar en Windows con Visual Studio 2022

Instala Visual Studio 2022 con la carga de trabajo **Desarrollo para el escritorio con C++**, CMake y Git. Instala vcpkg y define `VCPKG_ROOT` en PowerShell:

```powershell
$env:VCPKG_ROOT = "C:\vcpkg"
& "$env:VCPKG_ROOT\bootstrap-vcpkg.bat"
```

Desde esta carpeta del proyecto:

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64 `
  -DCMAKE_TOOLCHAIN_FILE="$env:VCPKG_ROOT\scripts\buildsystems\vcpkg.cmake"
cmake --build build --config Release
.
build\Release\proyectoGrafica_OpenGL.exe
```

La primera configuración descarga Assimp, GLEW, GLFW, GLM y stb con vcpkg. Se requiere conexión a internet la primera vez. También se puede abrir esta carpeta como proyecto CMake en Visual Studio.

## Controles

| Tecla/entrada | Acción |
| --- | --- |
| `Espacio` | Reproducir o pausar la secuencia completa |
| `R` | Regresar al inicio y pausar |
| `G` | Ir a la jugada de gol y reproducir desde ahí |
| `F` | Ir a la señal del árbitro y reproducir desde ahí |
| `1` / `2` | Cámara en primera persona / cámara de transmisión |
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
| Animación por transformaciones y huesos | Clips importados del GLB; reproducción, pausa y eventos para carrera, gol y señal del árbitro. |
| Interacción | Teclado, ratón, luces y marcador local. `G` y `F` permiten mostrar directamente las secuencias principales. |
| Sonido (opcional) | No incluido. |
| Memoria y Acta | Plantillas en `DOCUMENTACION_TECNICA.md` y `ACTA.md`; completar autores, capturas, video, explicación de zonas y resultados al ejecutar en Windows. |

## Estado de verificación

La escena GLB y los archivos de código están preparados. Falta compilar y recorrer la aplicación en una máquina con el toolchain de Windows y comprobar visualmente la posición de cámara, texturas, sombras y clips. La compilación y la prueba visual siguen pendientes en Windows; no se ha hecho commit ni push.
