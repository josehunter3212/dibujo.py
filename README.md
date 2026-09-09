import matplotlib.pyplot as plt
import numpy as np


def algoritmo_bresenham_circulo(xc, yc, r):
    """
    Calcula los puntos de un círculo usando el algoritmo de Bresenham.
    Aprovecha la simetría de 8 octantes del círculo.
    """
    x = 0
    y = r
    d = 3 - 2 * r
    puntos = set()

    def agregar_puntos_simetricos(xc, yc, x, y):
        # Añade los puntos correspondientes a los 8 octantes
        puntos.update([
            (xc + x, yc + y), (xc - x, yc + y),
            (xc + x, yc - y), (xc - x, yc - y),
            (xc + y, yc + x), (xc - y, yc + x),
            (xc + y, yc - x), (xc - y, yc - x)
        ])

    agregar_puntos_simetricos(xc, yc, x, y)

    while x <= y:
        x += 1
        # Evaluación del parámetro de decisión
        if d < 0:
            d = d + 4 * x + 6
        else:
            y -= 1
            d = d + 4 * (x - y) + 10
        agregar_puntos_simetricos(xc, yc, x, y)

    return puntos


def dibujar_cuadricula_y_circulo(xc, yc, r, tamaño_matriz=30):
    """
    Crea una matriz/cuadrícula visual y colorea los píxeles del círculo.
    """
    # Crear una matriz vacía llena de ceros (color de fondo)
    matriz = np.zeros((tamaño_matriz, tamaño_matriz))

    # Obtener los puntos del círculo mediante Bresenham
    puntos_circulo = algoritmo_bresenham_circulo(xc, yc, r)

    # Pintar los puntos en la matriz (validando que estén dentro de los límites)
    for px, py in puntos_circulo:
        if 0 <= px < tamaño_matriz and 0 <= py < tamaño_matriz:
            matriz[py, px] = 1  # Asignamos 1 para resaltar el píxel

    # Configuración de la visualización con Matplotlib
    fig, ax = plt.subplots(figsize=(8, 8))

    # Dibujar la matriz como píxeles (mapa de colores)
    ax.imshow(matriz, cmap="Blues", origin="lower")

    # Configurar la cuadrícula (Grid) exactamente sobre los límites de los píxeles
    ax.set_xticks(np.arange(-0.5, tamaño_matriz, 1), minor=True)
    ax.set_yticks(np.arange(-0.5, tamaño_matriz, 1), minor=True)
    ax.grid(which='minor', color='gray', linestyle='-', linewidth=0.5)

    # Etiquetas de los ejes
    ax.set_xticks(np.arange(0, tamaño_matriz, 5))
    ax.set_yticks(np.arange(0, tamaño_matriz, 5))
    ax.set_title(f"Círculo de Bresenham (Centro: {xc},{yc} | Radio: {r})", fontsize=14)

    # Resaltar el centro del círculo
    ax.plot(xc, yc, marker="o", color="red", label="Centro")
    ax.legend()

    plt.show()


# --- Configuración y ejecución ---
# Definimos el centro (xc, yc) y el radio (r)
centro_x = 15
centro_y = 15
radio = 10

# Ejecutar el dibujo
dibujar_cuadricula_y_circulo(centro_x, centro_y, radio, tamaño_matriz=32)

