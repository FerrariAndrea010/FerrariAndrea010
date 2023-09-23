- 👋 Hi, I’m @FerrariAndrea010
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- #pgzero
import random

WIDTH = 600
HEIGHT = 450

TITLE = "Viaggio spaziale"
FPS = 30

# Oggetti e variabili
ship = Actor("ship", (300, 400))
space = Actor("space")
enemies = []
planets = [Actor("plan1", (random.randint(0, 600), -100)), Actor("plan2", (random.randint(0, 600), -100)), Actor("plan3", (random.randint(0, 600), -100))]
meteors = []
bullets = []
mode = 'menu'
type1 = Actor("ship1", (100,200))
type2 = Actor("ship2", (300,200))
type3 = Actor("ship3", (500,200))

# Crea la lista di nemici
for i in range(5):
    x = random.randint(0, 600)
    y = random.randint(-450, -50)
    enemy = Actor("enemy", (x, y))
    enemy.speed = random.randint(2, 8)
    enemies.append(enemy)
    
# Crea la lista di meteore
for i in range(5):
    x = random.randint(0, 600)
    y = random.randint(-450, -50)
    meteor = Actor("meteor", (x, y))
    meteor.speed = random.randint(2, 10)
    meteors.append(meteor)

# Disegni
def draw():
# Avvio della schermata di menu
    if mode == 'menu':
        space.draw()
        screen.draw.text("Scegli la nave", center = (300,100), color = 'white', fontsize = 36)
        type1.draw()
        type2.draw()
        type3.draw()
    # Modalità di gioco
    if mode == 'game':
        space.draw()
        planets[0].draw()
        # Disegni meteore
        for i in range(len(meteors)):
            meteors[i].draw()
        ship.draw()
        # Disegni nemici
        for i in range(len(enemies)):
            enemies[i].draw()
            
        for i in range(len(bullets)):
            bullets[i].draw()
        
    # Finestra di game over
    elif mode == 'end':
        space.draw()
        screen.draw.text("GAME OVER!", center = (300, 200), color = "white", fontsize = 36)
    
# Controlli
def on_mouse_move(pos):
    ship.pos = pos

# Aggiungi nuovi nemici alla lista
def new_enemy():
    x = random.randint(0, 400)
    y = -50
    enemy = Actor("enemy", (x, y))
    enemy.speed = random.randint(2, 8)
    enemies.append(enemy)

# Movimento dei nemici
def enemy_ship():
    for i in range(len(enemies)):
        if enemies[i].y < 650:
            enemies[i].y = enemies[i].y + enemies[i].speed
        else:
            enemies.pop(i)
            new_enemy()

# Movimento dei pianeti
def planet():
    if planets[0].y < 550:
            planets[0].y = planets[0].y + 1
    else:
        planets[0].y = -100
        planets[0].x = random.randint(0, 600)
        first = planets.pop(0)
        planets.append(first)

# Movimento delle meteore
def meteorites():
    for i in range(len(meteors)):
        if meteors[i].y < 450:
            meteors[i].y = meteors[i].y + meteors[i].speed
        else:
            meteors[i].x = random.randint(0, 600)
            meteors[i].y = -20
            meteors[i].speed = random.randint(2, 10)


# Collisioni
def collisions():
    global mode
    for i in range(len(enemies)):
        if ship.colliderect(enemies[i]):
            mode = 'end'
        #Collisioni tra proiettili e nemici
        for j in range(len(bullets)):
            if bullets[j].colliderect(enemies[i]):
                enemies.pop(i)
                bullets.pop(j)
                new_enemy()
                break
        
def update(dt):
    if mode == 'game':
        enemy_ship()
        collisions()
        planet()
        meteorites()
# Movimento dei proiettili
        for i in range(len(bullets)):
            if bullets[i].y < 0:
                bullets.pop(i)
                break
            else:
                bullets[i].y = bullets[i].y - 10
    
        
        
def on_mouse_down(button, pos):
    global mode
    global ship
    if mode == "menu" and type1.collidepoint(pos):
        ship.image = "ship1"
        mode = "game"
    elif mode == "menu" and type2.collidepoint(pos):
        ship.image = "ship2"
        mode = "game"
    elif mode == "menu" and type3.collidepoint(pos):
        ship.image = "ship3"
        mode = "game"
        
    # creazione dei proiettili
    elif mode == "game" and button == mouse.LEFT:
        bullet = Actor("missiles")
        bullet.pos = ship.pos
        bullets.append(bullet)

<!---
FerrariAndrea010/FerrariAndrea010 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
