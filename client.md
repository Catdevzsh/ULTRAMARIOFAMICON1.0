import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up some constants
WIDTH, HEIGHT = 800, 600
PLAYER_SIZE = 50
ACCELERATION = 0.5
MAX_SPEED = 10
FRICTION = 0.9
JUMP_FORCE = 10
GRAVITY = 1

# NES-like palette
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (176, 48, 96)
GREEN = (0, 228, 48)

# Set up the player
player = pygame.Rect(WIDTH // 2, HEIGHT - PLAYER_SIZE - 10, PLAYER_SIZE, PLAYER_SIZE)
speed = 0
dy = 0
jumping = False

# Set up the ground
ground = pygame.Rect(0, HEIGHT - 10, WIDTH, 10)

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))

# Game loop
running = True
clock = pygame.time.Clock()  # Added clock for frame rate control
while running:
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE and not jumping:
                dy = -JUMP_FORCE
                jumping = True

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        speed -= ACCELERATION
    if keys[pygame.K_RIGHT]:
        speed += ACCELERATION

    # Apply friction
    speed *= FRICTION

    # Limit speed
    if speed > MAX_SPEED:
        speed = MAX_SPEED
    elif speed < -MAX_SPEED:
        speed = -MAX_SPEED

    # Update player position
    player.x += speed

    # Apply gravity
    dy += GRAVITY
    player.y += dy

    # Collision detection
    if player.colliderect(ground):
        player.y = ground.y - PLAYER_SIZE
        dy = 0
        jumping = False

    # Keep the player on the screen
    if player.left < 0:
        player.left = 0
    if player.right > WIDTH:
        player.right = WIDTH

    # Drawing
    screen.fill(BLACK)
    pygame.draw.rect(screen, RED, player)
    pygame.draw.rect(screen, GREEN, ground)

    # Flip the display
    pygame.display.flip()

    # Cap the frame rate at 60 FPS
    clock.tick(60)

# Quit Pygame
pygame.quit()
sys.exit()

## [FLAMES CO HUMMER TEAM STEAMOS 1.0 @Flames CO 2021-202XX]
