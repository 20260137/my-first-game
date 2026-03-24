import pygame
import sys
import random


pygame.init()


WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("2인 팩맨 미니 게임")


BLACK = (0, 0, 0)      # 배경
WHITE = (255, 255, 255) # 별
RED = (255, 0, 0)       # 플레이어 1
BLUE = (0, 0, 255)      # 플레이어 2
YELLOW = (255, 255, 0)  # 블록


radius = 20
speed = 5

red_x, red_y = WIDTH // 5, HEIGHT // 5
blue_x, blue_y = WIDTH * 4 // 5, HEIGHT // 5

red_active = True
blue_active = True


block_size = 30
num_blocks = 50
blocks = [
    pygame.Rect(random.randint(0, WIDTH - block_size),
                random.randint(0, HEIGHT - block_size),
                block_size, block_size)
    for _ in range(num_blocks)
]


stars = []
star_size = 15
STAR_INTERVAL = 2000
last_star_time = pygame.time.get_ticks()


font = pygame.font.SysFont("malgungothic", 72)


running = True
clock = pygame.time.Clock()
game_over = False


while running:
    clock.tick(60)

    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    if not game_over:
        keys = pygame.key.get_pressed()

        
        if red_active:
            if keys[pygame.K_a]:
                red_x -= speed
            if keys[pygame.K_d]:
                red_x += speed
            if keys[pygame.K_w]:
                red_y -= speed
            if keys[pygame.K_s]:
                red_y += speed

        
        if blue_active:
            if keys[pygame.K_LEFT]:
                blue_x -= speed
            if keys[pygame.K_RIGHT]:
                blue_x += speed
            if keys[pygame.K_UP]:
                blue_y -= speed
            if keys[pygame.K_DOWN]:
                blue_y += speed

        
        red_x = max(radius, min(WIDTH - radius, red_x))
        red_y = max(radius, min(HEIGHT - radius, red_y))
        blue_x = max(radius, min(WIDTH - radius, blue_x))
        blue_y = max(radius, min(HEIGHT - radius, blue_y))

        
        red_rect = pygame.Rect(red_x - radius, red_y - radius, radius * 2, radius * 2)
        blue_rect = pygame.Rect(blue_x - radius, blue_y - radius, radius * 2, radius * 2)

        
        for block in blocks[:]:
            if (red_active and red_rect.colliderect(block)) or \
               (blue_active and blue_rect.colliderect(block)):
                blocks.remove(block)

        
        if not blocks:
            game_over = True

        
        current_time = pygame.time.get_ticks()
        if current_time - last_star_time >= STAR_INTERVAL:
            star_x = random.randint(0, WIDTH - star_size)
            star_y = random.randint(0, HEIGHT - star_size)
            stars.append(pygame.Rect(star_x, star_y, star_size, star_size))
            last_star_time = current_time

        
        for star in stars:
            star.x += random.randint(-3, 3)
            star.y += random.randint(-3, 3)

            star.x = max(0, min(WIDTH - star_size, star.x))
            star.y = max(0, min(HEIGHT - star_size, star.y))

            if red_active and red_rect.colliderect(star):
                red_active = False

            if blue_active and blue_rect.colliderect(star):
                blue_active = False

        
        if not red_active and not blue_active:
            game_over = True

    
    screen.fill(BLACK)

    
    for block in blocks:
        pygame.draw.rect(screen, YELLOW, block)

    
    if red_active:
        pygame.draw.circle(screen, RED, (red_x, red_y), radius)
    if blue_active:
        pygame.draw.circle(screen, BLUE, (blue_x, blue_y), radius)

    
    for star in stars:
        pygame.draw.circle(
            screen,
            WHITE,
            (star.x + star_size // 2, star.y + star_size // 2),
            star_size // 2
        )

    
    if game_over:
        text = font.render("게임 종료!", True, WHITE)
        screen.blit(
            text,
            ((WIDTH - text.get_width()) // 2, HEIGHT // 2 - 40)
        )

    pygame.display.flip()

pygame.quit()
sys.exit()
