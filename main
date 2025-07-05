import pygame
import math
pygame.init()

class Player:
    def __init__(self, x, y, width=15, height=40, color=(185, 122, 87)):
        self.rect = pygame.Rect(x, y, width, height)
        self.color = color
        self.is_fall = False
        self.speed_y = 0

    def draw(self):
        pygame.draw.rect(win, self.color, self.rect )
    def move(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_d]:
            self.rect = self.rect.move(5, 0)
        if keys[pygame.K_a]:
            self.rect = self.rect.move(-5, 0)

        moving = self.rect.move(0, 1)
        all_platforms = platforms.copy()
        if platforms_draw:
            all_platforms.extend(platforms_draw)
        for platform in all_platforms:
            if platform.rect.colliderect(moving):
                self.rect.bottom = platform.rect.top
                return
        self.is_fall = True
        if self.speed_y < 5:
            self.speed_y += 1
        self.rect = self.rect.move(0, self.speed_y)
        for platform in all_platforms:
            if platform.rect.colliderect(moving):
                self.rect.bottom = platform.rect.top
                return


    def update(self):
        self.move()
        self.draw()


class Bullet:
    def __init__(self, x, y, x1, y1):
        self.rect = pygame.Rect(x, y, 1, 1)
class Enemy:
    def __init__(self, x, y, first_dot, second_dot, width=20, height=20, color=(254, 0, 0)):
        self.rect = pygame.Rect(x, y, width, height)
        self.color = color
        self.fir_dot = first_dot
        self.second_dot = second_dot
        self.x_speed = 1
        self.y_speed = 0

    def draw(self):
        pygame.draw.rect(win, self.color, self.rect)

    def move(self):

        if abs(self.second_dot[0] - self.rect.x) >= abs(self.second_dot[0] - self.rect.move(self.x_speed, 0).x):
            self.rect = self.rect.move(self.x_speed, self.y_speed)
        else:
            self.x_speed *= -1
            self.second_dot, self.fir_dot = self.fir_dot, self.second_dot
            # b = a
            # a = b
            # a, b = b, a

    def update(self):
        self.draw()
        self.move()


def get_angle(x, y, x1, y1):
        try:
            dy = abs(y - y1)
            dx = abs(x - x1)
            gip = (dx ** 2 + dy ** 2) ** 0.5
            angle = math.atan(dy / dx) * 57.3
            if x1 < x:
                angle = 180 - angle
            if y1 > y:
                angle = 360 - angle
            return angle
        except ZeroDivisionError:
            return angle


win = pygame.display.set_mode((500, 500))
run = True
player = Player(50, 0)
platforms = [Platforms(50, 80, 150),
             Platforms(100, 100)]
enemy = Enemy(80, 40, (50, 40), (450, 40))

platforms_draw = []
drawing = False
erase = False
paint = 700
font = pygame.font.Font(None, 32)
text = font.render(f"краска str(paint)",
                   False,
                   (255, 255, 255))

paint = 820


while run:
    mouse = pygame.mouse.get_pos()
    pygame.time.delay(30)
    for eve in pygame.event.get():
        if eve.type == pygame.QUIT:
            run = False
        if eve.type == pygame.MOUSEBUTTONDOWN:
            if eve.button == 1:
                drawing = True
            if eve.button == 3:
                erase = True
        if eve.type == pygame.MOUSEBUTTONUP:
            if eve.button == 1:
                drawing = False
            if eve.button == 3:
                erase = False
    mouse = pygame.mouse.get_pos() # (150, 125)
    if drawing and paint > 0:
        plat = Platforms(mouse[0], mouse[1])
        screen = pygame.Rect(0, 0, 500, 500)
        if screen.colliderect(plat.rect):
            platforms_draw.append(plat)
            paint -= 1

    if erase:
        eraser = pygame.Rect(*mouse, 10, 10)
        for platform in platforms_draw:
            if platform.rect.colliderect(eraser):
                platforms_draw.remove(platform)
                paint += 1
    text = font.render(f"краска {str(paint)}",
                       True,
                       (255, 255, 255))

    win.fill((0, 0, 0))
    for platform in platforms:
        platform.update()
    for platform in platforms_draw:
        platform.update()
    player.update()
    enemy.update()
    win.blit(text, (10, 10))
    pygame.display.update()
pygame.quit()
