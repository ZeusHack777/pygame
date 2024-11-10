Himport pygame
import time
import random

# Inicializa pygame
pygame.init()

# Colores
blanco = (255, 255, 255)
negro = (0, 0, 0)
rojo = (213, 50, 80)
verde = (0, 255, 0)
azul = (50, 153, 213)

# Tamaño de la pantalla
ancho = 600
alto = 400

pantalla = pygame.display.set_mode((ancho, alto))
pygame.display.set_caption('Snake')

reloj = pygame.time.Clock()
tamaño_serpiente = 10
velocidad_serpiente = 15

# Fuentes
fuente = pygame.font.SysFont("bahnschrift", 25)

def muestra_puntaje(puntaje):
    valor = fuente.render("Puntaje: " + str(puntaje), True, azul)
    pantalla.blit(valor, [0, 0])

def nuestra_serpiente(tamaño_serpiente, lista_serpiente):
    for x in lista_serpiente:
        pygame.draw.rect(pantalla, negro, [x[0], x[1], tamaño_serpiente, tamaño_serpiente])

def mensaje(msg, color):
    mensaje = fuente.render(msg, True, color)
    pantalla.blit(mensaje, [ancho / 6, alto / 3])

def juego():
    game_over = False
    game_close = False

    x1 = ancho / 2
    y1 = alto / 2

    x1_cambio = 0
    y1_cambio = 0

    lista_serpiente = []
    longitud_serpiente = 1

    comida_x = round(random.randrange(0, ancho - tamaño_serpiente) / 10.0) * 10.0
    comida_y = round(random.randrange(0, alto - tamaño_serpiente) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            pantalla.fill(blanco)
            mensaje("Perdiste! Presiona C para Jugar de Nuevo o Q para Salir", rojo)
            muestra_puntaje(longitud_serpiente - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        juego()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_cambio = -tamaño_serpiente
                    y1_cambio = 0
                elif event.key == pygame.K_RIGHT:
                    x1_cambio = tamaño_serpiente
                    y1_cambio = 0
                elif event.key == pygame.K_UP:
                    y1_cambio = -tamaño_serpiente
                    x1_cambio = 0
                elif event.key == pygame.K_DOWN:
                    y1_cambio = tamaño_serpiente
                    x1_cambio = 0

        if x1 >= ancho or x1 < 0 or y1 >= alto or y1 < 0:
            game_close = True
        x1 += x1_cambio
        y1 += y1_cambio
        pantalla.fill(blanco)
        pygame.draw.rect(pantalla, verde, [comida_x, comida_y, tamaño_serpiente, tamaño_serpiente])
        cabeza_serpiente = []
        cabeza_serpiente.append(x1)
        cabeza_serpiente.append(y1)
        lista_serpiente.append(cabeza_serpiente)
        if len(lista_serpiente) > longitud_serpiente:
            del lista_serpiente[0]

        for x in lista_serpiente[:-1]:
            if x == cabeza_serpiente:
                game_close = True

        nuestra_serpiente(tamaño_serpiente, lista_serpiente)
        muestra_puntaje(longitud_serpiente - 1)

        pygame.display.update()

        if x1 == comida_x and y1 == comida_y:
            comida_x = round(random.randrange(0, ancho - tamaño_serpiente) / 10.0) * 10.0
            comida_y = round(random.randrange(0, alto - tamaño_serpiente) / 10.0) * 10.0
            longitud_serpiente += 1

        reloj.tick(velocidad_serpiente)

    pygame.quit()
    quit()

juego()
