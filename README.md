# Guia_ECDI-11_1
Guia campos vectoriales
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

#Funcion Campos direccionales 


def campo_direccional(ecuacion):

    h = 0.5  # separación de la malla

    m = np.arange(-10, 11)
    n = np.arange(-10, 11)

    X, Y = np.meshgrid(m * h, n * h)

    pendiente = ecuacion(X, Y)

    norm = np.sqrt(1 + pendiente**2)

    U = 1 / norm
    V = pendiente / norm

    fig, ax = plt.subplots()

    ax.quiver(
        X, Y, U, V,
        angles='xy',
        color='navy'
    )

    ax.set_xlim(-5, 5)
    ax.set_ylim(-5, 5)

    ax.set_title(
        f"Campo Direccional de ecuación {nombres_ecuaciones[ecuacion]}"
    )

    plt.show()


#Funcion familia de soluciones 
def familia_soluciones(ecuacion):

    # Separación de la malla
    h = 0.5

    m = np.arange(-10, 11)
    n = np.arange(-10, 11)

    X, Y = np.meshgrid(m * h, n * h)

    # Calcular las pendientes
    pendiente = ecuacion(X, Y)

    # Normalizar los vectores
    norm = np.sqrt(1 + pendiente**2)

    U = 1 / norm
    V = pendiente / norm

    # Crear gráfica
    fig, ax = plt.subplots()

    # Vectores del campo direccional
    ax.quiver(
        X, Y, U, V,
        angles='xy',
        color='navy'
    )

    # Valores iniciales para la familia
    valores_iniciales = [-3, -2, -1, 0, 1, 2, 3]

    # Intervalo
    x = np.linspace(-5, 5, 500)

    # Resolver para cada condición inicial
    for y0 in valores_iniciales:

        solucion = solve_ivp(
            ecuacion,
            [-5, 5],
            [y0],
            t_eval=x
        )

        ax.plot(
            solucion.t,
            solucion.y[0]
        )

    # Configuración
    ax.set_xlim(-5, 5)
    ax.set_ylim(-5, 5)

    ax.set_xlabel("x")
    ax.set_ylabel("y")


    ax.set_title(
        f"Familias solución de ecuación {nombres_ecuaciones[ecuacion]}"
    )
    ax.grid()

    plt.show()

#Funcion problema de valor inicial 
def problema_valor_inicial(ecuacion, x0, y0):


    izquierda = x0 - 3
    derecha = x0 + 3


    h = 0.5

    m = np.arange(
        np.floor(izquierda / h),
        np.ceil(derecha / h) + 1
    )

    n = np.arange(-10, 11)

    X, Y = np.meshgrid(m * h, n * h)

    # Calcular pendientes
    pendiente = ecuacion(X, Y)

    # Normalizar
    norm = np.sqrt(1 + pendiente**2)

    U = 1 / norm
    V = pendiente / norm


    fig, ax = plt.subplots()

    # Campo de direcciones
    ax.quiver(
        X, Y, U, V,
        angles='xy',
        color='navy'
    )

    x_derecha = np.linspace(x0, derecha, 500)

    solucion_derecha = solve_ivp(
        ecuacion,
        [x0, derecha],
        [y0],
        t_eval=x_derecha
    )


    x_izquierda = np.linspace(x0, izquierda, 500)

    solucion_izquierda = solve_ivp(
        ecuacion,
        [x0, izquierda],
        [y0],
        t_eval=x_izquierda
    )


    ax.plot(
        solucion_izquierda.t,
        solucion_izquierda.y[0],
        linewidth=3
    )

    ax.plot(
        solucion_derecha.t,
        solucion_derecha.y[0],
        linewidth=3
    )

   

    ax.scatter(
        x0,
        y0,
        s=70,
        zorder=5
    )


    ax.set_xlim(izquierda, derecha)

    ax.set_xlabel("x")
    ax.set_ylabel("y")

    ax.set_title(
        f"Solución de ecuación {nombres_ecuaciones[ecuacion]} "
        f"para el punto ({x0}, {y0})"
    )

    ax.grid()

    plt.show()

# Ecuaciones 

ecu_a_1 = lambda x, y: -y - np.sin(x)

ecu_b_1 = lambda x, y: x + y

ecu_c_1 = lambda x, y: -x**2 + np.sin(y)

ecu_d_1 = lambda x, y: (6*x - 3*x*y) / (x**2 + 1)

ecu_e_1 = lambda x, y: x * np.exp(y)

ecu_f_1 = lambda x, y: x - y

nombres_ecuaciones = {
    ecu_a_1: "1(a) y' = -y - sin(x)",
    ecu_b_1: "1(b) y' = x + y",
    ecu_c_1: "1(c) y' = -x² + sin(y)",
    ecu_d_1: "1(d) (x² + 1)y' + 3xy = 6x",
    ecu_e_1: "1(e) y' = x e^y",
    ecu_f_1: "1(f) y' = x - y"
}

#Punto 1 

#a)

campo_direccional(ecu_a_1)
familia_soluciones(ecu_a_1)
problema_valor_inicial(ecu_a_1, 0, 1)

#b) 
campo_direccional(ecu_b_1)
familia_soluciones(ecu_b_1)
problema_valor_inicial(ecu_b_1, -2, 2)

#c) 
campo_direccional(ecu_c_1)
familia_soluciones(ecu_c_1)

#d) 

campo_direccional(ecu_d_1)
familia_soluciones(ecu_d_1)

#e)
campo_direccional(ecu_e_1)
familia_soluciones(ecu_e_1)

#f
campo_direccional(ecu_f_1)
familia_soluciones(ecu_f_1)
problema_valor_inicial(ecu_f_1, 1, 1)
