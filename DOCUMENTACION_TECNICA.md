# Memoria técnica — Proyecto de gráfica

> Borrador vinculado a `Proyecto Final 2027-1`. Completar los campos entre corchetes y añadir capturas reales de la aplicación compilada. No entregar este borrador sin revisar los resultados.

## 1. Datos del proyecto

- Deporte y escenario: fútbol; [nombre del escenario]
- Integrantes: [nombres]
- Plataforma de ejecución: Windows 10/11, Visual Studio 2022, x64
- API gráfica: OpenGL 3.3 Core, GLSL 330

## 2. Organización de la escena

La escena se modeló y organizó en Blender. Se exportó como `assets/escenario.glb`, conservando mallas, materiales, texturas, dos esqueletos y las animaciones disponibles. La aplicación carga el archivo en ejecución con Assimp.

Las tres zonas principales distinguidas en el modelo son:

1. **Cancha**: césped, líneas, áreas y círculo central.
2. **Gradas**: nivel inferior y nivel superior alrededor del campo.
3. **Accesos y plaza exterior**: entradas, fachada y pavimento perimetral.

Los nodos exportados incluyen el jugador, el árbitro, la portería, el balón, cuatro banderines en las esquinas, autobús, carrito, marcador y reflectores. Añadir una captura general y acercamientos que demuestren su ubicación y visibilidad.

## 3. Arquitectura del programa

- GLFW crea la ventana y recoge teclado, ratón y rueda.
- GLEW inicializa las funciones de OpenGL.
- Assimp importa las mallas, jerarquías, materiales, texturas embebidas, pesos de piel y clips de animación desde glTF/GLB.
- GLM calcula matrices de cámara, proyección y transformación.
- Los shaders GLSL aplican texturas e iluminación por fragmento.
- El HUD dibuja el marcador en pantalla.

## 4. Animaciones e interacción

Los clips principales del GLB son `Jugador CHICHARITO Carrera.004`, `Balon_GolAction.005` y `Arbitro Señal de falta.004`. La aplicación evalúa canales de posición, rotación y escala y matrices de hueso para deformar las mallas. `Espacio` reproduce/pausa; `G` inicia la secuencia del gol y `F` la señal del árbitro. `J` y `K` modifican el marcador.

Documentar aquí los fotogramas/tiempos reales observados tras la prueba y adjuntar capturas o enlace al video de demostración: [pendiente].

## 5. Cámaras e iluminación

La cámara de primera persona permite caminar y mirar alrededor. La cámara de transmisión orbita el área de juego. La proyección es perspectiva. `N` alterna la apariencia diurna/nocturna; `L` conmuta los cuatro focos en modo noche. Se calcula un mapa de profundidad direccional de 2048×2048 cada fotograma y se aplica un filtrado PCF de 3×3 en GLSL. Confirmar visualmente el resultado en el equipo de entrega.

## 6. Compilación y ejecución

Seguir los comandos de `README.md` con Visual Studio 2022 x64 y vcpkg. Registrar aquí la versión de Windows, el GPU, la configuración compilada y el resultado real:

- Resultado de compilación: [pendiente]
- Resultado de ejecución: [pendiente]
- Problemas observados y correcciones: [pendiente]

## 7. Metodología y comparación

Describir el flujo Blender → exportación GLB → carga en Assimp → evaluación de animación → renderizado OpenGL. Comparar el resultado con la escena original de Blender usando capturas equivalentes. Comentar diferencias en iluminación, materiales, orientación, escala, animación y rendimiento: [pendiente].

## 8. Reparto de trabajo

| Integrante | Responsabilidad | Evidencia |
| --- | --- | --- |
| [nombre] | [modelado/animación/programación/documentación] | [captura o archivo] |

## 9. Evidencias por requisito

Añadir capturas con rótulos: escena completa, tres zonas, personajes, texturas, cámara primera persona, cámara transmisión, día/noche, luces, marcador y secuencias de carrera/gol/árbitro. Añadir el video de demostración solicitado por la consigna.
