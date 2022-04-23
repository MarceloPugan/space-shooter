# space-shooter
just space shooter game


import pygame
import random

pygame.init()

x = 1280
y = 720

screen = pygame.display.set_mode((x,y))
pygame.display.set_caption('spaceship')

bg = pygame.image.load('cenario.png').convert_alpha()
bg = pygame.transform.scale(bg, (x, y))

navevermelho = pygame.image.load('objetovermelho.png').convert_alpha()
navevermelho = pygame.transform.scale(navevermelho, (40, 40))

naveazul = pygame.image.load('objetoazul2d.png').convert_alpha()
naveazul = pygame.transform.scale(naveazul, (40, 40))

tiro = pygame.image.load('objetoamarelo.png').convert_alpha()
tiro = pygame.transform.scale(tiro, (10, 10))

tiro2 = pygame.image.load('tirosecundario.png').convert_alpha()
tiro2 = pygame.transform.scale(tiro, (20, 20))

pos_alien_x = 500
pos_alien_y = 360

pos_jogador_x = 200
pos_jogador_y = 300

vel_tiro_x = 0
pos_tiro_x = 300
pos_tiro_y = 300

vel_tiro2_x = 0
pos_tiro2_x = 300
pos_tiro2_y = 300

pontos = 10

triggered = False

rodando = True

font = pygame.font.SysFont('fonts/PixelGameFont.ttf', 50)

player_rect = naveazul.get_rect()
alien_rect = navevermelho.get_rect()
tiro_rect = tiro.get_rect()
tiro2_rect = tiro2.get_rect()

def respawn():
    x = 1350
    y = random.randint(1, 640)
    return [x, y]

def respawn_tiro():
    triggered = False
    respawn_tiro_x = pos_jogador_x 
    respawn_tiro_y = pos_jogador_y 
    vel_tiro_x = 0
    return [respawn_tiro_x, respawn_tiro_y, triggered, vel_tiro_x]


def respawn_tiro2():
    triggered = False
    respawn_tiro2_x = pos_jogador_x 
    respawn_tiro2_y = pos_jogador_y 
    vel_tiro2_x = 0
    return [respawn_tiro2_x, respawn_tiro2_y, triggered, vel_tiro2_x]

def colisions():
    global pontos
    if player_rect.colliderect(alien_rect): # or alien_rect.x == 40:
        pontos -=1
        return True
    if tiro_rect.colliderect(alien_rect):
        pontos +=1
        return True
    else:
        return False    

while rodando:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            rodando = False

    screen.blit(bg, (0,0))

    rel_x = x % bg.get_rect().width
    screen.blit(bg, (rel_x - bg.get_rect().width,0))      
    if rel_x < 1280:
        screen.blit(bg, (rel_x, 0))


    tecla = pygame.key.get_pressed()
    if tecla[pygame.K_UP] and pos_jogador_y > 1:
        pos_jogador_y -= 1
        if not triggered:
            pos_tiro_y -= 1
    if tecla[pygame.K_DOWN] and pos_jogador_y < 680:
        pos_jogador_y += 1
        if not triggered:
            pos_tiro_y += 1
    if tecla[pygame.K_LEFT] and pos_jogador_x > 1:
        pos_jogador_x -= 1
        if not triggered:
            pos_tiro_x -= 1
    if tecla[pygame.K_RIGHT] and pos_jogador_x < 1240:
        pos_jogador_x += 1
        if not triggered:
            pos_tiro_x += 1
    if tecla[pygame.K_LCTRL]:
        triggered = True  
        vel_tiro_x = 10
    if tecla[pygame.K_SPACE]:
        triggered = True
        vel_tiro2_x = 7

    if pontos == -1:
        rodando = False

    if pos_alien_x == 20:
        pos_alien_x = respawn()[0]
        pos_alien_y = respawn()[1]


    if pos_tiro_x == 1300:
        pos_tiro_x, pos_tiro_y, triggered, vel_tiro_x = respawn_tiro()

    if pos_tiro2_x == 1300:
        pos_tiro2_x, pos_tiro2_y, triggered, vel_tiro2_x = respawn_tiro2()


    if pos_alien_x == 20 or colisions():
        pos_alien_x = respawn()[0]
        pos_alien_y = respawn()[1]


    player_rect.y = pos_jogador_y
    player_rect.x = pos_jogador_x

    tiro_rect.y = pos_tiro_y
    tiro_rect.x = pos_tiro_x

    tiro2_rect.y = pos_tiro2_y
    tiro2_rect.x = pos_tiro2_x

    alien_rect.y = pos_alien_y
    alien_rect.x = pos_alien_x


    x -= 5
    pos_alien_x -= 2

    pos_tiro_x += vel_tiro_x

    pos_tiro2_x += vel_tiro2_x

    score = font.render(f' pontos: {int(pontos)}', True, (0, 0, 0))
    screen.blit(score, (50,50))



    pygame.draw.rect(screen, (255, 0, 0), player_rect, 4 )
    pygame.draw.rect(screen, (255, 0, 0), tiro_rect, 4 )
    pygame.draw.rect(screen, (255, 0, 0), tiro2_rect, 4 )
    pygame.draw.rect(screen, (255, 0, 0), alien_rect, 4 )


    screen.blit(navevermelho, (pos_alien_x, pos_alien_y))
    screen.blit(tiro, (pos_tiro_x,pos_tiro_y))
    screen.blit(naveazul, (pos_jogador_x,pos_jogador_y))


    print(pontos)



    pygame.display.update()    
