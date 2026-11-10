# Memoria técnica — Proyecto de gráfica

> Borrador vinculado a `Proyecto Final 2027-1`. Completar los campos entre corchetes y añadir capturas reales de la aplicación compilada. No entregar este borrador sin revisar los resultados.

## 1. Datos del proyecto

- Deporte y escenario: fútbol; [nombre del escenario]
- Integrantes: [nombres]
- Plataforma de ejecución: Windows 10/11, Visual Studio 2022, Win32 (x86)
- API gráfica: OpenGL 3.3 Core, GLSL 330

## 2. Organización de la escena

La escena se modeló y organizó en Blender. Se exportó como `assets/escenario.glb`, conservando mallas, materiales, texturas, dos esqueletos y las animaciones disponibles. La aplicación carga el archivo en ejecución con Assimp.

Las tres zonas principales distinguidas en el modelo son:

1. **Cancha**: césped, líneas, áreas y círculo central.
2. **Gradas**: nivel inferior y nivel superior alrededor del campo.
3. **Accesos y plaza exterior**: entradas, fachada y pavimento perimetral.

Los nodos exportados incluyen el jugador, el árbitro, porterías, balón, cuatro banderines, autobús, carrito de alimentos, vehículo de emergencias, marcador, mascota, trofeo, podio, bancas, reflectores, gradas y accesos. El programa instancia el rig del jugador para formar 11 participantes por equipo. Añadir capturas reales que demuestren su ubicación y visibilidad en Windows.

## 3. Arquitectura del programa

- GLFW crea la ventana y recoge teclado, ratón y rueda.
- GLEW inicializa las funciones de OpenGL.
- Assimp importa las mallas, jerarquías, materiales, texturas embebidas, pesos de piel y clips de animación desde glTF/GLB. Las texturas se convierten a RGBA antes de subirlas a OpenGL para conservar un formato de píxel consistente.
- GLM calcula matrices de cámara, proyección y transformación.
- Los shaders GLSL aplican texturas e iluminación por fragmento.
- El HUD dibuja el marcador en pantalla.

## 4. Animaciones e interacción

La aplicación inicia automáticamente una secuencia de 20 segundos. Primero sigue desde arriba el autobús acercándose al estadio; después corta a una vista aérea de los 22 jugadores y el árbitro en el centro; continúa con la carrera y el balón entrando en la portería, la señal del árbitro y el aumento automático del marcador; al marcar, el marcador tiene un pulso de escala; al final entra el vehículo de emergencias, se acerca a un jugador y ambos salen hacia el exterior. La formación reutiliza el rig del modelo cargado y las mallas con piel; una instancia cambia de posición para representar el traslado. Las luces y ruedas se dibujan desde los modelos existentes. `Espacio` pausa/reanuda, `R` vuelve al principio, `G` salta al gol y `F` a la señal arbitral. `1`, `2` y `3` cambian las cámaras.

Documentar aquí los fotogramas/tiempos reales observados tras la prueba y adjuntar capturas o enlace al video de demostración: [pendiente].

### 4.1 Correcciones de colocación posteriores a la primera prueba

La primera ejecución mostró varios errores de escala y ubicación en la exportación GLB. Se corrigieron en la escena, conservando la animación esquelética del jugador y el árbitro:

- **Jugador:** el canal de traslación del clip `Control | jugador carreraAction.003` se retargeteó para recorrer el eje longitudinal desde `x=-36` hasta `x=-48`, con `z=8`. Así permanece dentro de los límites de la cancha (`x=±52.5`) y termina cerca del área de gol. La cámara de transmisión sigue la misma trayectoria.
- **Árbitro:** su raíz queda en el centro del campo para iniciar entre ambos equipos; la tecla `F` permite revisar la señal.
- **Carrito de alimentos:** las mallas de origen estaban expresadas a una escala desproporcionada; la raíz se escala a `0.05` y se coloca en el corredor lateral opuesto, fuera de la ruta del vehículo médico. El vehículo de emergencias se escala a `0.22` en tiempo de ejecución y entra desde el acceso exterior para llegar a la banda y retirarse con un jugador.
- **Porterías:** se construyeron dos marcos de aproximadamente `7.32 × 2.44 m` con retícula de red. Se ubican en `x=-52.5` y `x=52.5`, centrados en `z=0`. La portería izquierda conserva el balón animado. Se orientó el clip para que el balón avance desde el campo, cruce la línea y termine dentro de la red.
- **Banderines:** sus cuatro raíces se alinean con las esquinas del campo en `(±52.5, ±34)`.
- **Luminarias:** las cuatro torres se reconstruyeron como mástiles verticales de unos `22 m` con cabezales de focos y se colocaron en el perímetro, aproximadamente en `(±82, ±52)`. Los focos dinámicos apuntan hacia el centro y se activan con `N`/`L`.

El script `tools/ajustar_escena.py` documenta los cambios de GLB sobre la exportación original. Se ejecutó para producir la escena ajustada; no volver a ejecutarlo sobre esa escena porque agrega nodos auxiliares. La secuencia y las nuevas instancias requieren compilar y revisar visualmente en Windows. Ajustar posiciones/escala si la VM muestra penetraciones o solapamientos.

## 5. Cámaras e iluminación

La cámara de primera persona permite caminar y mirar alrededor. La cámara de transmisión sigue al jugador. La cámara cinematográfica hace cortes entre la toma de dron, la vista de equipos, el gol y el vehículo. La proyección es perspectiva. `N` alterna la apariencia diurna/nocturna; `L` conmuta los cuatro focos. Se calcula un mapa de profundidad direccional de 2048×2048 cada fotograma y se aplica un filtrado PCF de 3×3 en GLSL. Confirmar visualmente el resultado en el equipo de entrega.

## 6. Compilación y ejecución

Seguir los comandos de `README.md` con Visual Studio 2022 Win32 (x86) y vcpkg. Si se cierra inesperadamente, revisar `startup.log` junto al ejecutable. Registrar aquí la versión de Windows, el GPU, la configuración compilada y el resultado real:

- Resultado de compilación: el flujo de Windows terminó correctamente para Win32 (x86).
- Resultado de ejecución: pendiente de volver a probar en la VM después de normalizar las texturas a RGBA. La compilación anterior terminaba con una violación de acceso durante la carga del GLB; la nueva versión agrega `startup.log` para registrar la fase exacta si vuelve a ocurrir.
- Entorno de prueba real (versión de Windows, GPU/VM): [completar después de probar].
- Resultado visual y problemas restantes: [completar con observaciones y capturas de la VM].

## 7. Metodología y comparación

Se propone Kanban, con columnas Pendiente, En proceso, Revisión y Terminado. Las tareas se dividen por entregable (modelado, exportación, integración, animaciones, controles, documentación y pruebas), y cada ajuste se revisa en la escena antes de cerrarse. Es adecuado para el equipo pequeño y para las correcciones iterativas de integración que ha requerido el proyecto.

**Comparación:** Scrum aporta iteraciones planificadas y reuniones de seguimiento, pero puede introducir más coordinación de la necesaria para un equipo de 2–3 personas. Cascada facilita ordenar fases y fechas, pero los cambios que aparecen al integrar modelos se devuelven con más coste a etapas anteriores. Kanban mantiene el trabajo visible y permite priorizar continuamente los bloqueos y correcciones.

**Etapas documentadas:**

1. **Planeación:** elegir fútbol soccer, listar requisitos del PDF, identificar modelos existentes y separar tareas. Falta registrar la aprobación del profesor, integrantes y fechas reales.
2. **Diseño:** organizar cancha, zona técnica/servicio y gradas/accesos; establecer la secuencia de autobús, partido, gol y atención médica; definir controles y cámaras.
3. **Implementación:** preparar y corregir el GLB en Blender; cargarlo con Assimp; implementar renderizado, texturas, sombras, cámaras, animaciones por huesos/transformaciones, marcador e interacción en C++/OpenGL.
4. **Pruebas:** compilar Win32 mediante el flujo de Windows; ejecutar en la VM; revisar visualmente personajes, posiciones, animaciones, luces, sombras, marcador y controles. La compilación está confirmada; la ejecución visual posterior a la corrección de texturas aún debe registrarse.

La comparativa visual final debe mostrar capturas equivalentes de Blender y OpenGL. Comentar cambios observados de materiales, orientación, escala, iluminación, animación y rendimiento: [completar después de la prueba en VM].

## 8. Reparto de trabajo

| Integrante | Responsabilidad | Evidencia |
| --- | --- | --- |
| [Nombre 1] | Coordinación e integración (ajustar a trabajo real) | [captura o archivo] |
| [Nombre 2] | Modelado, animación y exportación (ajustar a trabajo real) | [captura o archivo] |
| [Nombre 3, si aplica] | Programación OpenGL y pruebas (ajustar a trabajo real) | [captura o archivo] |

## 9. Evidencias por requisito

Añadir capturas con rótulos: escena completa, tres zonas, 11 contra 11 y árbitro, texturas, cámara primera persona, cámara de transmisión, toma cinematográfica, día/noche, reflectores, marcador antes/después del gol y secuencia del vehículo médico. Añadir el video de demostración solicitado por la consigna y la comparativa con Blender. No sustituir estas evidencias por imágenes de referencia: deben capturarse de la ejecución real.
