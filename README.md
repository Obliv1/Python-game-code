# Python-game-code

import pygame

#Initialise pygame
pygame.init()

#Create the game window
Width = 800
Length = 600
Window = pygame.display.set_mode((Width, Length))

#Characters position, size and changes in velocity
char_Width = 40
char_Length = 80
char_X = 20
char_Y = 520
delta_X = 1
char_X_change = 0

jump_velocity = 10
gravity = 0.9

counter = 0
jump = False



running = True
while running:
    #Background colour
    Window.fill((255,0,0))
    
    for event in pygame.event.get():
        #Closes game if you click exit on the game window
        if event.type == pygame.QUIT:
            running = False
            pygame.quit()
    #Moves character when you press keys down
    keys = pygame.key.get_pressed()
    if keys[pygame.K_SPACE]:
            jump = True
    if keys[pygame.K_LEFT]:
        char_X_change = -delta_X
    else:
        if keys[pygame.K_RIGHT]:
            char_X_change = delta_X
        else:
            char_X_change=0

        
    #Stop character from going out of screen
    #For x-axis
    if char_X < 0:
        char_X = 0
    elif char_X + char_Width > Width:
        char_X = Width - char_Width
    #For y-axis
    elif char_Y < 0:
        char_Y = 0
    elif char_Y + char_Length > Length:
        char_Y = Length - char_Length

    #Makes player jump while not below screen
    if jump and ((Length - char_Length - (jump_velocity*counter - gravity*(counter)**2/2)) <= Length - char_Length) :
        char_Y = ((Length - char_Length - (jump_velocity*counter - gravity*(counter)**2/2)))
        counter += 0.1
    if ((Length - char_Length - (jump_velocity*counter - gravity*(counter)**2/2))) > (Length - char_Length):
        jump = False
        counter = 0
        char_Y = Length - char_Length

    #Collision with the green rectangle
    if (char_X + char_Width > 400) and (char_X < 500) and (char_Y + char_Length < Length) and (char_Y + char_Length > Length - 50):
        char_Y = Length - char_Length - 50
        jump = False
        #counter = 0
    if (char_X + char_Width > 400) and (char_X + char_Width < 410) and (char_Y + char_Length > Length - 50):
        char_X = 400 - char_Width
    if (char_X < 500) and (char_X > 490) and (char_Y + char_Length > Length - 50):
        char_X = 500 
        
            
    #Updates the X position of player
    char_X += char_X_change
    pygame.draw.rect(Window, (0,0,255), (char_X,char_Y,char_Width,char_Length))
    pygame.draw.rect(Window, (0,255,50), (400,550,100,50))
    pygame.display.update()

