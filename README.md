# game
i cant make mous control for shooting
import pygame as pg
import pygame
import random
import math
from variables import*

font_name = pygame.font.match_font("arial")
TITLE = "tank game"
vec = pygame.math.Vector2
PLAYER_ACC = 0.5
PLAYER_FRIC = -0.12
M = 1
WID = 900
HEI = 900
FPS = 60
SPEED = 10
x = WID/2
MOB_ACC = 0.5
MOB_FRIC = -0.08
BULLET_SPEED = 10
score = 0
POWER_UP_ACC = 0.2
POWER_UP_FRIC = -0.12
game_ower = True
spred = 3
shoot_delay = 250


def draw_text(surf, text, size, x, y):
    font = pg.font.Font(font_name, size)
    text_surface = font.render(text, True, (0, 255, 255))
    text_rect = text_surface.get_rect()
    text_rect.midtop = (x, y)
    surf.blit(text_surface, text_rect,)


def draw_shild_bar(surf, x, y, pct):
    if pct < 0:
        pct = 0

    BAR_HEIGHT = 5
    fill = g.player.shild

    fill_rect = pg.Rect(x, y, fill, BAR_HEIGHT)
    pg.draw.rect(g.scr, (225, 0, 0), fill_rect)


class Player(pg.sprite.Sprite):
    def __init__(self):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((30, 30))
        self.image.fill((225, 225, 225))
        self.rect = self.image.get_rect()
        self.rect.center = (WID/2, HEI/2)
        self.rect.y = HEI/2
        self.pos = vec(WID/2, HEI/2)
        self.vel = vec(0, 0)
        self.acc = vec(0, 0)
        self.shoot_delay = 1
        self.last_shoot = pygame.time.get_ticks()
        self.spred = 1
        self.shild = 20

    def update(self):
        self.acc = vec(0, 0)
        keystate = pg.key.get_pressed()
        if keystate[pg.K_a]:
            self.acc.x = -PLAYER_ACC
        if keystate[pg.K_d]:
            self.acc.x = PLAYER_ACC
        self.acc += self.vel*PLAYER_FRIC
        self.vel += self.acc
        self.pos += self.vel+0.5*self.acc
        self.rect.center = self.pos
        if keystate[pg.K_w]:
            self.acc.y = -PLAYER_ACC
        if keystate[pg.K_s]:
            self.acc.y = PLAYER_ACC
        self.acc += self.vel*PLAYER_FRIC
        self.vel += self.acc
        self.pos += self.vel+0.5*self.acc
        if self.pos.x > WID:
            self.pos.x = 0
        if self.pos.x < 0:
            self.pos.x = WID
        if self.pos.y > HEI:
            self.pos.y = 0
        if self.pos.y < 0:
            self.pos.y = HEI
        self.rect.center = self.pos
        mousestate = pg.mouse.get_pressed()
        if mousestate[1]:
            self.shoot1()
        if keystate[pg.K_DOWN]:
            self.shoot1()
        if keystate[pg.K_LEFT]:
            self.shoot3()
        if keystate[pg.K_RIGHT]:
            self.shoot4()

    def shoot1(self):
        now = pygame.time.get_ticks()
        if now-self.last_shoot > self.shoot_delay:
            self.last_shoot = now
            mx, my = pg.mouse.get_pos()
            bullet = Bullet1(self.rect.centerx, self.rect.centery, mx, my)
            g.all_sprites.add(bullet)
            g.bullets.add(bullet)

    def shoot2(self):
        now = pygame.time.get_ticks()
        if now-self.last_shoot > self.shoot_delay:
            self.last_shoot = now
            bullet = Bullet2(self.rect.centerx, self.rect.centery)
            g.all_sprites.add(bullet)
            g.bullets.add(bullet)

    def shoot3(self):
        now = pygame.time.get_ticks()
        if now-self.last_shoot > self.shoot_delay:
            self.last_shoot = now
            bullet = Bullet3(self.rect.centerx, self.rect.centery)
            g.all_sprites.add(bullet)
            g.bullets.add(bullet)

    def shoot4(self):
        now = pygame.time.get_ticks()
        if now-self.last_shoot > self.shoot_delay:
            self.last_shoot = now
            bullet = Bullet4(self.rect.centerx, self.rect.centery)
            g.all_sprites.add(bullet)
            g.bullets.add(bullet)


class Bullet1(pg.sprite.Sprite):
    def __init__(self, x, y, x2, y2):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((10, 10))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed = BULLET_SPEED
        self.x2 = x2
        self.y2 = y2
      #X2,Y2=pg.mouse.get_pos()
    def update(self):
        KOK = 1
        if self.x2 > self.y2:
            KOK = abs((self.x2-g.player.rect.centerx)/10)
        if self.x2 < self.y2:
            KOK = abs((g.player.rect.centery-self.y2)/10)
        if KOK == 0:
            KOK = 1
        self.rect.x += (self.x2-g.player.rect.centerx) / KOK
        self.rect.y -= (g.player.rect.centery-self.y2) / KOK
        if self.rect.left < 0:
            self.kill()
        if self.rect.top > HEI:
            self.kill()
        if self.rect.right > WID:
            self.kill()
        if self.rect.bottom < 0:
            self.kill()
        print(KOK)


class Bullet2(pg.sprite.Sprite):
    def __init__(self, x, y):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((10, 10))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed = BULLET_SPEED

    def update(self):

        self.rect.x += random.randrange(-g.player.spred, g.player.spred)
        self.rect.y += self.speed
        if self.rect.top > HEI:
            self.kill()


class Bullet3(pg.sprite.Sprite):
    def __init__(self, x, y):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((10, 10))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed = BULLET_SPEED

    def update(self):
        self.rect.y += random.randrange(-g.player.spred, g.player.spred)
        self.rect.x -= self.speed
        if self.rect.left < 0:
            self.kill()


class Bullet4(pg.sprite.Sprite):
    def __init__(self, x, y):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((10, 10))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed = BULLET_SPEED

    def update(self):
        self.rect.y += random.randrange(-g.player.spred, g.player.spred)
        self.rect.x += self.speed
        if self.rect.right > WID:
            self.kill()


class Mob(pg.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image_orig = pg.Surface((40, 40))
        self.image_orig.fill((225, 225, 0))
        self.image = self.image_orig.copy()
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(0-WID, WID)
        self.rect.y = random.randrange(0-HEI, HEI)
        self.speedy = random.randrange(2, 5)
        self.rot = 0
        self.rot_speed = 8
        self.last_update = pg.time.get_ticks()

        if self.rect.x > 0:
            self.rect.x += WID
        if self.rect.y > 0:
            self.rect.y += HEI
        self.pos = vec(self.rect.x, self.rect.y)
        self.vel = vec(0, 0)
        self.acc = vec(0, 0)

    def update(self):
        if self.rect.y < g.player.rect.y:
            self.acc.y = MOB_ACC
        if self.rect.y > g.player.rect.y:
            self.acc.y = -MOB_ACC
        if self.rect.x < g.player.rect.x:
            self.acc.x = MOB_ACC
        if self.rect.x > g.player.rect.x:
            self.acc.x = -MOB_ACC
        self.acc += self.vel*MOB_FRIC
        self.vel += self.acc
        self.pos += self.vel+0.5*self.acc
        self.rect.center = self.pos


class power_up(pg.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image_orig = pg.Surface((40, 40))
        self.image_orig.fill((225, 225, 0))
        self.image = self.image_orig.copy()
        self.rect = self.image.get_rect()
        self.rect.x = g.mob.rect.x
        self.rect.y = g.mob.rect.y
        self.speedy = random.randrange(2, 5)
        self.rot = 0
        self.rot_speed = 8
        self.last_update = pg.time.get_ticks()
        if self.rect.x > 0:
            self.rect.x += WID
        if self.rect.y > 0:
            self.rect.y += HEI
        self.pos = vec(self.rect.x, self.rect.y)
        self.vel = vec(0, 0)
        self.acc = vec(0, 0)

    def update(self):
        if self.rect.y > g.player.rect.y:
            self.acc.y = POWER_UP_ACC
        if self.rect.y < g.player.rect.y:
            self.acc.y = -POWER_UP_ACC
        if self.rect.x > g.player.rect.x:
            self.acc.x = POWER_UP_ACC
        if self.rect.x < g.player.rect.x:
            self.acc.x = -POWER_UP_ACC
        self.acc += self.vel*POWER_UP_FRIC
        self.vel += self.acc
        self.pos += self.vel+0.5*self.acc
        self.rect.center = self.pos


class Game:
    def __init__(self):
        # initialize game window
        pg.init()
        pg.mixer.init()
        self.scr = pg.display.set_mode((WID, HEI))
        pygame.display.set_caption("tank crash")
        self.clock = pg.time.Clock()
        self.running = True
        self.player = Player()
        self.mobs = pg.sprite.Group()
        self.all_sprites = pg.sprite.Group()
        self.bullets = pg.sprite.Group()
        self.mobs = pg.sprite.Group()
        self.player = Player()
        self.mob = Mob()
        self.all_sprites.add(self.player)
        # self.all_sprites.add(self.mob)
        # self.mobs.add(self.mob)

    def new(self):
        # start a new game
        self.all_sprites = pg.sprite.Group()
        self.bullets = pg.sprite.Group()
        self.mobs = pg.sprite.Group()
        self.player = Player()
        self.mob = Mob()
        self.power = power_up()
        self.all_sprites.add(self.player)
        self.all_sprites.add(self.mob)
        self.mobs.add(self.mob)
        for i in range(1):
            self.mob = Mob()
            self.all_sprites.add(self.mob)
            self.mobs.add(self.mob)
        self.run()

    def run(self):
        # game loop
        self.playing = True
        while self.playing:
            self.clock.tick(FPS)
            self.events()
            self.update()
            self.draw()
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    self.running = False

    def update(self):
        # game loop-update
        global score
        global spred
        global shoot_delay
        self.all_sprites.update()
        self.hits = pg.sprite.spritecollide(self.player, self.mobs, True)
        for hit in self.hits:
            self.player.shild -= 0
            m = Mob()
            self.all_sprites.add(m)
            self.mobs.add(m)
            if self.player.shild <= 0:
                self.playing = False
        hits = pg.sprite.groupcollide(self.mobs, self.bullets, True, True)
        for hit in hits:
            self.player.shoot_delay -= 20
            g.player.spred += 1
            if spred >= 30:
                spred = 30
            score += 1
            g.mob.rect.x
            for i in range(random.randrange(1, 3)):
                m = Mob()
                self.all_sprites.add(m)
                self.mobs.add(m)

    def events(self):
        # game loop-events
        for event in pg.event.get():
            if event.type == pg.QUIT:
                if self.playing:
                    self.playing = False
                self.running = False
            if event.type == pg.MOUSEBUTTONDOWN:
                self.player.shoot1()

    def draw(self):
        # game loop-draw
        self.scr.fill((0, 0, 0))
        self.all_sprites.draw(self.scr)
        mx, my = pg.mouse.get_pos()
        pg.draw.line(self.scr, (255, 255, 255), (self.player.rect.centerx,
                                                 self.player.rect.centery), (mx, my), 1)
        pg.draw.circle(self.scr, (255, 255, 255), (self.player.rect.centerx,
                                                   self.player.rect.centery), 30, 1)
        draw_shild_bar(self.scr, self.player.rect.centerx - 10,
                       self.player.rect.centery+25, self.player.shild)
    # vzdy posledne
        pg.display.flip()

    def show_start_screen(self):
        # start screen
        draw_text(self.scr, "TANK CRASH", 64, WID//2, HEI//4)
        draw_text(self.scr, "W,A,S,D to move,Arrow keys to shoot", 25, WID//2, HEI//2)
        draw_text(self.scr, "press key to start", 35, WID//2, HEI//4+HEI//2)

        pg.display.flip()
        wating = True
        while wating:
            self.clock.tick(FPS)
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    self.running = False
                if event.type == pg.KEYDOWN:
                    wating = False

    def show_go_screen(self):
        # game ower
        draw_text(self.scr, "Game over", 64, WID//2, HEI//4)

        draw_text(self.scr, "press key to play egen", 35, WID//2, HEI//4+HEI//2)
        draw_text(self.scr, "score: " + str(score), 35, WID//2, HEI//3)
        pg.display.flip()
        wating = True
        while wating:
            self.clock.tick(FPS)
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    wating = False
                    self.running = False
                if event.type == pg.KEYDOWN:
                    wating = False


g = Game()
g.show_start_screen()
while g.running:
    g.new()
    g.show_go_screen()
pg.quit()
