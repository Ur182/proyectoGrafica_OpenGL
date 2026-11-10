# Proyecto de gráfica: escenario deportivo OpenGL

Repositorio con la documentación, una copia del código C++ principal y un ZIP con el proyecto completo para compilar.

## Archivos

- `DOCUMENTACION_TECNICA.md`: memoria técnica y matriz de requisitos.
- `ACTA.md`: acta para completar con los datos y firmas reales.
- `main.cpp`: copia individual del código C++ principal.
- `proyectoGrafica_OpenGL.zip`: proyecto completo, con CMake, dependencias, shaders, escena GLB ajustada y código fuente organizado.

## Ajustes de escena

La última revisión corrige la escala del carrito, mantiene al jugador dentro de la cancha, coloca al árbitro en el área, alinea los cuatro banderines con las esquinas, incluye porterías en ambos extremos y reconstruye las cuatro torres de luminarias. `G` activa la jugada del gol y `F` la señal del árbitro. El ZIP contiene el detalle y el script de ajustes.

## Clonar y compilar

```powershell
git clone https://github.com/Ur182/proyectoGrafica_OpenGL.git
```

Después de clonar, extrae `proyectoGrafica_OpenGL.zip`. Abre la carpeta extraída en Visual Studio 2022 con **File > Open > Folder** y sigue el README incluido en el ZIP para instalar dependencias, compilar en x64 y ejecutar.

GitHub Actions compila y empaqueta una versión Windows x64 cada vez que se actualiza el ZIP. La compilación del paquete anterior pasó; esta escena ajustada aún debe compilarse y revisarse visualmente en Windows.
