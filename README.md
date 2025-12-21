# PingPongGame
Ping Pong
from pygame import *
'''Required classes'''


# Parent class for sprites
class GameSprite(sprite.Sprite):
    # Class constructor
    def __init__(self, player_image, player_x, player_y, player_speed, wight, height):
        # Add two more parameters when creating the sprite and manually set
        # the size of the rectangle for the image
        super().__init__()
 
        # Each sprite must have an image property
        self.image = transform.scale(image.load(player_image), (wight, height))
        self.speed = player_speed
 
        # Each sprite must have a rect property (the rectangle it fits into)
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y


    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))


# Child class for the player sprite (controlled by keys)
class Player(GameSprite):
    '''
    keys = {
        K_a: True,
        K_s: False,
        K_d: True,
        K_SPACE: False,
        K_LEFT: True,
        K_UP: True
    }
    '''
    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed


    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
       
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed


# Game scene:
back = (200, 255, 255)  # Background color
win_width = 600
win_height = 500
window = display.set_mode((win_width, win_height))
window.fill(back)


# Flags responsible for the game state
game = True
finish = False
clock = time.Clock()
FPS = 60


# Creating the ball and paddles
racket1 = Player('racket.png', 30, 200, 4, 50, 150)
racket2 = Player('racket.png', 520, 200, 4, 50, 150)
ball = GameSprite('tenis_ball.png', 200, 200, 4, 50, 50)


font.init()
font = font.Font(None, 35)
lose1 = font.render('PLAYER 1 LOSE!', True, (180, 0, 0))
lose2 = font.render('PLAYER 2 LOSE!', True, (180, 0, 0))


speed_x = 3
speed_y = 3


while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
   
    if finish != True:
        window.fill(back)
        racket1.update_l()
        racket2.update_r()
        ball.rect.x += speed_x
        ball.rect.y += speed_y


        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            speed_x *= -1
     
       
        # If the ball reaches the top or bottom of the screen, reverse direction
        if ball.rect.y > win_height - 50 or ball.rect.y < 0:
            speed_y *= -1


        # If the ball goes past the left paddle, player 1 loses
        if ball.rect.x < 0:
            finish = True
            window.blit(lose1, (200, 200))
            game_over = True


        # If the ball goes past the right paddle, player 2 loses
        if ball.rect.x > win_width:
            finish = True
            window.blit(lose2, (200, 200))
            game_over = True


        racket1.reset()
        racket2.reset()
        ball.reset()


    display.update()
    clock.tick(FPS)
