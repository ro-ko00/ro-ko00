import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the screen
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Simple Shooter Game")

# Define colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Define player properties
player_width = 50
player_height = 50
player_x = screen_width // 2 - player_width // 2
player_y = screen_height - player_height - 10
player_speed = 5

# Bullet properties
bullet_width = 5
bullet_height = 10
bullet_speed = 7
bullets = []

# Enemy properties
enemy_width = 50
enemy_height = 50
enemy_speed = 3
enemies = []

# Create the player
player = pygame.Rect(player_x, player_y, player_width, player_height)

# Game loop flag
running = True

# Main game loop
while running:
    screen.fill(BLACK)

    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Get the keys pressed
    keys = pygame.key.get_pressed()

    # Move player
    if keys[pygame.K_LEFT] and player.x > 0:
        player.x -= player_speed
    if keys[pygame.K_RIGHT] and player.x < screen_width - player_width:
        player.x += player_speed
    if keys[pygame.K_DOWN] and player.y < screen_height - player_height:
        player.y += player_speed
    if keys[pygame.K_UP] and player.y > 0:
        player.y -= player_speed

    # Shoot bullets
    if keys[pygame.K_SPACE]:
        bullet = pygame.Rect(player.x + player_width // 2 - bullet_width // 2, player.y, bullet_width, bullet_height)
        bullets.append(bullet)

    # Move bullets
    for bullet in bullets[:]:
        bullet.y -= bullet_speed
        if bullet.y < 0:
            bullets.remove(bullet)

    # Create enemies randomly
    if random.randint(1, 50) == 1:
        enemy_x = random.randint(0, screen_width - enemy_width)
        enemy = pygame.Rect(enemy_x, 0, enemy_width, enemy_height)
        enemies.append(enemy)

    # Move enemies
    for enemy in enemies[:]:
        enemy.y += enemy_speed
        if enemy.y > screen_height:
            enemies.remove(enemy)

    # Check for collisions
    for enemy in enemies[:]:
        for bullet in bullets[:]:
            if enemy.colliderect(bullet):
                enemies.remove(enemy)
                bullets.remove(bullet)
                break

    # Draw player, bullets, and enemies
    pygame.draw.rect(screen, WHITE, player)
    for bullet in bullets:
        pygame.draw.rect(screen, RED, bullet)
    for enemy in enemies:
        pygame.draw.rect(screen, (0, 255, 0), enemy)

    # Update the screen
    pygame.display.flip()

    # Set the game speed
    pygame.time.Clock().tick(60)

# Quit Pygame
pygame.quit()
