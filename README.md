import pygame
import threading
import time
import random



white = (255,255,255)
black = (0,0,0)
red = (255,0,0)
green = (0,255,0)
blue = (0,0,255)
screen=pygame.display.set_mode((600,600))
screen_rect=screen.get_rect()
pygame.display.set_caption('Pong')
left_x = 60
left_y = 150
right_x = 540
right_y = 150
ball_x = 190
ball_y = 190
ballMH = "R"
ballMV = "U"
pygame.key.set_repeat(1, 10)
speed = 0.5


p1_score = 0
p2_score = 0
def message(msg,msg2, black):
    font = pygame.font.SysFont(None, 60)
    text = font.render(msg, True, black)
    screen.blit(text, [420, 10])
    text2 = font.render(msg2, True, black)
    screen.blit(text2, [350, 10])
    pygame.display.update()
    


def Ball():
    
    
    
    global ballMH, ball_x, ball_y, ballMV, p1_score, p2_score, speed
    ball = pygame.draw.rect(screen, red, [ball_x, ball_y, 10, 10])
    jj = random.randint(0,600)
    jb = random.randint(0,600)
    
    if ballMH == "R":  
        ball_x += speed
    if ballMH == "L": 
        ball_x -= speed
    if ballMV == "D":  
        ball_y -= speed
        if ball_y < 10:
            ballMV = "U"
    if ballMV == "U":
        ball_y += speed
        if ball_y > 590:
            ballMV = "D"
    if ball_x > 590:
        ball_x = 11
        ball_y = jj
        #ballMV = None
        p2_score += 1
        time.sleep(0.3)
        message(str(p1_score),str(p2_score), black)
        time.sleep(0.3)
        pass
    if ball.colliderect(player1) == True:
        ballMH = "L"
        speed += 0.01
        #ballMV = "U"
    if  ball.colliderect(player) == True:
        #ballMV = "D"
        ballMH = "R"
        speed += 0.01
    if ball_x < 10:
        ball_x = 585
        ball_y = jj
        #ballMV = None
        p1_score += 1
        time.sleep(0.3)
        message(str(p1_score),str(p2_score), black)
        time.sleep(0.3)
        pass
class paddles():
    def __init__(self, pos2, pos3):        
        
        global player, player1
        player = pygame.draw.rect(screen, black, [left_x, pos3,10,100]) 
        player1 = pygame.draw.rect(screen, black, [right_x, pos2,10,100])
        player.clamp_ip(screen_rect) # ensure player is inside screen
        player1.clamp_ip(screen_rect) # ensure player is inside screen
        pygame.draw.rect(screen, (0,0,0), player)
        pygame.draw.rect(screen, (0,0,0), player1)
        
        



def Main():
    pygame.init()
    
    gameExit = False
    global left_y  
    global right_y  
    #t = threading.Thread(target=Ball())
    
    while not gameExit:
        ball = pygame.draw.rect(screen, red, [ball_x, ball_y, 10, 10])
        
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                gameExit = True
            if event.type == pygame.KEYDOWN: 
                keys = pygame.key.get_pressed()
    
                if keys[pygame.K_w]: 
                    if right_y >= 10:right_y -= 10
                    if ball.colliderect(player) == True:
                        ballMV = "U"
                if keys[pygame.K_s]: 
                    if right_y <= 500:right_y += 10
                    if ball.colliderect(player) == True:
                        ballMV = "D"
                if keys[pygame.K_UP]: 
                    if left_y >= 10:left_y -= 10
                    if ball.colliderect(player1) == True:
                        ballMV = "U"
                if keys[pygame.K_DOWN]:
                    if left_y <= 500:left_y += 10
                    if ball.colliderect(player1) == True:
                        ballMV = "D"
                      
        
        screen.fill(white)
        line = pygame.draw.rect(screen, blue, [295, 0, 10, 600] )  
        paddles(left_y, right_y)
        Ball()      
        message(str(p1_score),str(p2_score), black)
        
        pygame.display.update()
    
    

if __name__=='__main__': 
    Main()
    pygame.quit()
    quit()
 
