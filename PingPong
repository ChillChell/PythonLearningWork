from pygame import *
import random
#импортируем функцию для засекания времени, 
#чтобы интерпретатор не искал эту функцию в pygame модуле time, даём ей другое название сами
from time import time as timer 
 
#фоновая музыка
mixer.init()
plob_sound = mixer.Sound('pong.ogg')

#Картинки
img_bg = "Background.jpeg" #фон игры
img_pl = "Paddle.png" #Ракетка
img_ball = "Ball.png" #Мяч
speed_x = 12
speed_y = 15

#класс-родитель для других спрайтов
class GameSprite(sprite.Sprite):
 #конструктор класса
   def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
       #вызываем конструктор класса (Sprite):
       sprite.Sprite.__init__(self)
 
       #каждый спрайт должен хранить свойство image - изображение
       self.image = transform.scale(image.load(player_image), (size_x, size_y))
       self.speed = player_speed
 
       #каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
       self.rect = self.image.get_rect()
       self.rect.x = player_x
       self.rect.y = player_y
 #метод, отрисовывающий героя на окне
   def reset(self):
       window.blit(self.image, (self.rect.x, self.rect.y))
 
#класс главного игрока
class Player(GameSprite):
   #метод для управления спрайтом стрелками клавиатуры
   def update(self):
       keys = key.get_pressed()
       if keys[K_w] and self.rect.y > 5:
           self.rect.y -= self.speed
       if keys[K_s] and self.rect.y < win_height - 175:
           self.rect.y += self.speed
   def updateR(self):
       keys = key.get_pressed()
       if keys[K_UP] and self.rect.y > 5:
           self.rect.y -= self.speed
       if keys[K_DOWN] and self.rect.y < win_height - 175:
           self.rect.y += self.speed

class Ball(GameSprite):
    def movement(self):
        global speed_x, speed_y
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        if ball.rect.y > win_height - 50 or ball.rect.y < 0:
            mixer.Sound.play(plob_sound)
            speed_y *= -1
        if sprite.collide_rect(player, ball) or sprite.collide_rect(opponent, ball):
            speed_x *= -1
            mixer.Sound.play(plob_sound)
        if ball.rect.x < 0 or ball.rect.x > win_width:
            ball.rect.x = 350
            ball.rect.y = 250
            #Метод Choice случайно выбирает одно из двух значений, для того чтобы мяч при появление в исходной точки летел в разные направления.
            speed_x *= random.choice((-1,1))
            speed_y *= random.choice((-1,1))

#создаем окошко
win_width = 700
win_height = 500
display.set_caption("ПингПонг")
window = display.set_mode((win_width, win_height))
background = transform.scale(image.load(img_bg), (win_width, win_height))

 
#создаем спрайты
player = Player(img_pl, 20, 210, 15, 150, 15)
opponent = Player(img_pl, 600, 210, 15, 150, 15)
ball = Ball(img_ball, 350 , 250, 80, 35, 25)


#переменная "игра закончилась": как только там True, в основном цикле перестают работать спрайты
finish = False
#основной цикл игры:
run = True #флаг сбрасывается кнопкой закрытия окна
while run:
   #событие нажатия на кнопку Закрыть
    for e in event.get():
        if e.type == QUIT:
            run = False
    if not finish:
        #обновляем фон
        window.blit(background,(0,0))

        #производим движения спрайтов
        player.update()
        opponent.updateR()
        ball.movement()
    
        #обновляем их в новом местоположении при каждой итерации цикла
        player.reset()
        opponent.reset()
        ball.reset()

        display.update()
    #цикл срабатывает каждую 0.05 секунд
    time.delay(50)
