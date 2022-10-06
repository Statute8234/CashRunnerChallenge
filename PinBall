from ursina import *
import random,time

def update():
    global timer,money
    Money.text = 'Money ${:.2f}'.format(money)
    Timer.text = 'Timer {}'.format(timer)
    Timer.background = True
    if timer > 0 and play == True:
        timer -= 1
    if timer <= 0:
        money = round(money * 0.13,2)
        restart()

start_game = False
timer = 200

def restart():
    global play,start_game,timer
    timer = 200
    
    play = False
    start_game = False
    for pon in pon_list:
        pon.position = (random.uniform(-7,7),4)
        pon.speed = 0.01
        pon.clickable = True
        pon.color = color.white

    for box in box_list:
        box.color = color.blue
        box.clickable = True
        box.active = False
        box.money = round(random.uniform(0.01,99),2)
        box.text = '${:.2f}'.format(box.money)

    for polls in polls_list:
         polls.position = (random.uniform(-7,7),random.uniform(-2.5,2.5))
         
    line = Line(position = (0,3.8))
    line.scale = (100,0.05)

    red_circle = Red_Circle(position = (0,0.05))

polls_list = []
class Polls(Button):
    def __init__(self, position=(0,0,0)):
        super().__init__()
        self.parent = scene
        self.position = position
        self.model = "circle"
        self.color = color.random_color()
        self.scale = 0.1
        self.texture = 'white_cube'
        
    def update(self):
        hit_info = self.intersects()
        if hit_info.hit:
            if hit_info.entity in polls_list:
                if hit_info.entity.x > self.x:
                    self.x -= 0.01
                if hit_info.entity.x < self.x:
                    self.x += 0.01
                

money = 0
class Red_Circle(Button):
    def __init__(self, position=(0,0,0)):
        super().__init__()
        global money
        self.parent = camera.ui
        self.position = position
        self.model = "circle"
        self.color = color.red
        if money <= 0:
            self.money = round(random.uniform(0.01,99),2)
        else:
            self.money = money
        self.text = 'Your starting with: ${:.2f}'.format(self.money)
        self.scale = 0.7

    def update(self):
        global play
        for pon in pon_list:
            if pon.clickable == False:
                destroy(self)
                play = True
            if play == True:
                pon.speed = 0.01

    def input(self, key):
        global money,start_game
        if self.hovered:
            if key == 'left mouse down':
                start_game = True
                money = self.money
                destroy(self)

box_list = []      
class Box(Button):
    def __init__(self, position=(0,0,0)):
        super().__init__()
        self.parent = scene
        self.position = position
        self.model = "quad"
        self.texture = 'white_cube'
        self.color = color.blue
        self.collider = 'box'
        self.money = round(random.uniform(0.01,99),2)
        self.clickable = True
        self.active = False
        self.text = '${:.2f}'.format(self.money)

    def update(self):    
        # ability to see highlight
        if self.clickable == False:
            self.highlight_color = self.color
        else:
            self.highlight_color = color.black

    def input(self, key):
        if self.hovered:
            if key == 'left mouse down' and self.clickable == True:
                self.active = True
                self.clickable = False
                self.color = color.red

                for box in box_list:
                    box.clickable = False

play = False
class Line(Entity):
    def __init__(self, position=(0,0,0)):
        super().__init__()
        self.parent = scene
        self.position = position
        self.model = "quad"
        self.texture = 'white_cube'
        self.color = color.red
        self.collider = 'box'
        self.clickable = True

    def update(self):
        global play
        for pon in pon_list:
            if pon.clickable == False:
                destroy(self)
                play = True
            if play == True:
                pon.speed = random.uniform(0.05,0.1)

pon_list = []
class Pon(Button):
    def __init__(self, position=(0,0,0)):
        super().__init__()
        self.parent = scene
        self.position = position
        self.model = "sphere"
        self.texture = 'white_cube'
        self.color = color.white
        self.clickable = True
        self.scale = 0.1
        self.speed = 0.01

    def update(self):
        global money
        # ability to see highlight
        if self.clickable == False or start_game == False:
            self.highlight_color = self.color
        else:
            self.highlight_color = color.black
        # hit info 
        hit_info = self.intersects()
        if hit_info.hit:
            # hit polls
            if hit_info.entity in polls_list:
                if random.randint(0,1) == 0:
                    self.x += 0.3
                else:
                    self.x -= 0.3
            # hit the boxes
            if hit_info.entity in box_list:
                if self.color == color.orange:
                    if hit_info.entity.active == True:
                        money += (hit_info.entity.money * 2)
                        time.sleep(1)
                        restart()
                    else:
                        money += hit_info.entity.money
                        time.sleep(1)
                        restart()
            # hit other pons
            if hit_info.entity in pon_list:
                if hit_info.entity.x > self.x:
                    self.x -= 0.01
                if hit_info.entity.x < self.x:
                    self.x += 0.01
        else:
            self.y -= self.speed
            if self.y <= -5 and self.color == color.orange:
                money -= round(money / 2,2)
                restart()

    def input(self, key):
        if self.hovered:
            if key == 'left mouse down' and self.clickable == True and start_game == True:
                self.clickable == False
                self.color = color.orange

                for pon in pon_list:
                    pon.clickable = False
                
app = Ursina()
red_circle = Red_Circle(position = (0,0.05))
Money = Text(text = 'Money ${:.2f}'.format(money),y = 0.45,x = - 0.1)
Timer = Text(text = 'Timer ${:.2f}'.format(timer),y = 0.4,x = - 0.8)

for x in range(100):
    polls = Polls(position = (random.uniform(-7,7),random.uniform(-2.5,3.5)))
    polls_list.append(polls)

for x in range(7):
    x *= 2
    box = Box(position = (x - 6,-3.5))
    box_list.append(box)

for x in range(100):
    pon = Pon(position = (random.uniform(-7,7),4))
    pon_list.append(pon)
    
line = Line(position = (0,3.8))
line.scale = (100,0.05)
app.run()
