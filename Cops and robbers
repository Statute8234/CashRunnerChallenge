import pygame,random,time
from pygame.locals import *
pygame.init()
# 1520,770
screen_width,screen_height = 1520,770
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.flip()
background = pygame.image.load(r"C:\Users\daniel\Downloads\Gear-Google-Maps-Future-1066713660.webp")
background_transform = pygame.transform.scale(background,(screen_width, screen_height))
mapX,mapY = 0,0

bank_list = []
class Bank(pygame.sprite.Sprite):
    def __init__(self):
        self.width = 50
        self.height = 50
        self.x = screen_width - 50
        self.y = screen_height - 50
        self.image = pygame.image.load(r"C:\Users\daniel\Downloads\bank.png")
        self.image_transform = pygame.transform.scale(self.image,(self.width,self.height))
        self.rect = self.image.get_rect()
        # hit box
        self.hitbox = (self.x,self.y,self.width,self.height)

    def draw(self):
        global max_speed
        for bank in bank_list:
            if bank.hitbox[0] <= player.x <= bank.hitbox[0] + bank.hitbox[2] and bank.hitbox[1] <= player.y <= bank.hitbox[1] + bank.hitbox[3]:
                if player.holding_money == False:
                    player.move_time = 1000
                    player.holding_money = True
                    self.move()
                
        screen.blit(self.image_transform, (self.x,self.y))
                
    def move(self):
        self.x,self.y = random.randint(50,screen_width - 50),random.randint(50,screen_height - 50)
        self.hitbox = (self.x,self.y,self.width,self.height)

Return_list = []
class Return_Money(pygame.sprite.Sprite):
    def __init__(self):
        self.width = 50
        self.height = 50
        self.x = 0
        self.y = 0
        self.image = pygame.image.load(r"C:\Users\daniel\Downloads\house.png")
        self.image_transform = pygame.transform.scale(self.image,(self.width,self.height))
        self.rect = self.image.get_rect()
        # hit box
        self.hitbox = (self.x,self.y,self.width,self.height)

    def draw(self):
        global max_speed
        for Return in Return_list:
            if Return.hitbox[0] <= player.x <= Return.hitbox[0] + Return.hitbox[2] and Return.hitbox[1] <= player.y <= Return.hitbox[1] + Return.hitbox[3]:
                if player.holding_money == True:
                    self.move()
                    max_speed += 0.05
                    player.holding_money = False
        # -----------------------------
        for ememies in ememies_list:
            ememies.speed = random.uniform(0.01,max_speed)
        screen.blit(self.image_transform, (self.x,self.y))
                
    def move(self):
        self.x,self.y = random.randint(50,screen_width - 50),random.randint(50,screen_height - 50)
        self.hitbox = (self.x,self.y,self.width,self.height)
    

money_icon = pygame.image.load(r"C:\Users\daniel\Downloads\money-bag.png")
money_icon_transform = pygame.transform.scale(money_icon,(50, 50))
class Player(pygame.sprite.Sprite):
    def __init__(self,x,y,width,height):
        pygame.sprite.Sprite.__init__(self)
        self.move_time = 0
        self.holding_money = True
        self.ability_move = True
        self.width = width
        self.height = height
        self.x = x
        self.y = y
        self.speed = 0.5
        self.angle = 0
        self.image = pygame.image.load(r"C:\Users\daniel\Downloads\kindpng_890065.png")
        self.image_transform = pygame.transform.scale(self.image,(self.width,self.height))
        self.image_rotate = pygame.transform.rotate(self.image_transform, self.angle)
        self.rect = self.image.get_rect()
        # hit box
        self.hitbox = (self.x,self.y,self.width,self.height)

    def hit(self):
        for ememies in ememies_list:
            if ememies.hitbox[0] <= player.x <= ememies.hitbox[0] + ememies.hitbox[2] and ememies.hitbox[1] <= player.y <= ememies.hitbox[1] + ememies.hitbox[3]:
                restart()
        
    def move(self):
        if self.ability_move == True:
            userInput = pygame.key.get_pressed()
            if userInput[pygame.K_LEFT]:
                self.angle = 90
                self.x -= self.speed
            if userInput[pygame.K_RIGHT]:
                self.angle = -90
                self.x += self.speed
            if userInput[pygame.K_UP]:
                self.angle = 0
                self.y -= self.speed
            if userInput[pygame.K_DOWN]:
                self.angle = -180
                self.y += self.speed
            self.image_rotate = pygame.transform.rotate(self.image_transform, self.angle)

        self.rect.x = self.x
        self.rect.y = self.y
        
    def draw(self):
        if self.move_time > 0:
            self.ability_move = False
            self.move_time -= 1
        else:
            self.ability_move = True

        if self.holding_money == True:
            screen.blit(money_icon_transform, (self.x,self.y - 50))
        # -----------------------
        self.move()
        self.hit()
        screen.blit(self.image_rotate, (self.x,self.y))
        self.hitbox = (self.x,self.y,self.width,self.height)

ememies_list = []
class Ememies(pygame.sprite.Sprite):
    def __init__(self,x,y,width,height):
        global max_speed
        pygame.sprite.Sprite.__init__(self)
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.speed = random.uniform(0.01,max_speed)
        self.angle = -90
        self.image = pygame.image.load(r"C:\Users\daniel\Downloads\kindpng_1508997.png")
        self.image_transform = pygame.transform.scale(self.image,(self.width,self.height))
        self.image_rotate = pygame.transform.rotate(self.image_transform, self.angle)
        self.rect = self.image.get_rect()
        # hit box
        self.hitbox = (self.x,self.y,self.width,self.height)
        
    def hit(self):
        for ememies in ememies_list:
            if player.hitbox[0] <= ememies.x + ememies.width <= player.hitbox[0] + player.hitbox[2] and player.hitbox[1] <= ememies.y + ememies.height <= player.hitbox[1] + player.hitbox[3]:
                restart()

    def move_angle(self):
        self.angle = (player.angle + 90)
        self.image_rotate = pygame.transform.rotate(self.image_transform, self.angle)
        
    def move(self):
        self.move_angle()
        if self.y + 5 > player.y:
            self.y -= self.speed
        if self.y + 5 < player.y:
            self.y += self.speed
        if self.x + 5 > player.x:
            self.x -= self.speed
        if self.x + 5 < player.x:
            self.x += self.speed

        self.rect.x = self.x
        self.rect.y = self.y
        
    def draw(self):
        self.move()
        self.hit()
        screen.blit(self.image_rotate, (self.x,self.y))
        self.hitbox = (self.x,self.y,self.width,self.height)

def restart():
    global max_speed
    player.x,player.y = screen_width - 20,screen_height - 30
    for ememies in ememies_list:
        ememies.x = random.randint(40,screen_width - 40)
        ememies.y = random.randint(20,screen_height - 20)
        ememies.speed = ememies.speed
        
    for bank in bank_list:
        bank.move()
    # -------------------------------------
    for return_money in Return_list:
        return_money.move()

    max_speed += 0.01

# players
player = Player(screen_width - 20,screen_height - 30,20,30)

max_speed = 0.01
for x in range(5):
    ememies_x,ememies_y = random.randint(40,screen_width - 40),random.randint(20,screen_height - 20)
    ememies = Ememies(ememies_x,ememies_y,40,30)
    ememies_list.append(ememies)

return_money = Return_Money()
bank = Bank()

bank_list.append(bank)
Return_list.append(return_money)
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            
    screen.fill((255,255,255))
    screen.blit(background_transform, (mapX,mapY))
    
    for bank in bank_list:
        bank.draw()

    for return_money in Return_list:
        return_money.draw()

    player.draw()

    for ememies in ememies_list:
        ememies.draw()
        
    # hit info
    pygame.display.update()
