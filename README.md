
---

### ✅ Agora é só:

1. Criar um repositório no GitHub  
2. Fazer `git init`, adicionar os arquivos, dar commit e push  

---

Se quiser, posso te ajudar a fazer o **passo a passo pra publicar no GitHub**. Quer que eu faça isso?
python jogo.py
pygame
# 🔥 Fogo de Luta Livre 🔥

Jogo de luta 1x1 feito em Python com Pygame.

## Como jogar:

- Jogador 1: **A / D** para mover, **ESPACO** para atacar
- Jogador 2: **← / →** para mover, **ENTER** para atacar

## Instalação:

```bash
pip install pygame
fogo-de-luta-livre/
├── README.md
├── requirements.txt
└── jogo.py
import pygame
import sys

# Inicialização do Pygame
pygame.init()

# Configurações da Tela
LARGURA, ALTURA = 900, 600
TELA = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("🔥 Fogo de Luta Livre 🔥")

# Cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
CINZA = (100, 100, 100)
AMARELO = (255, 255, 0)

# Cores para os personagens
CORES = [
    (255, 0, 0), (0, 0, 255), (0, 255, 0), (255, 255, 0), (150, 0, 255),
    (255, 128, 0), (128, 0, 255), (255, 50, 50), (50, 50, 255), (50, 255, 50),
    (200, 200, 50), (200, 50, 200), (50, 200, 200), (150, 75, 0), (75, 0, 150),
    (100, 100, 100), (255, 100, 180), (180, 255, 100), (100, 180, 255), (255, 255, 255)
]

# Fonte
fonte = pygame.font.SysFont("Arial", 24)

# FPS
clock = pygame.time.Clock()
FPS = 60

# Lista de personagens
personagens = [
    "Denis Smash Fire", "Ismael Big Ice", "Lucas Thunder Bolt", "Ana Shadow Queen",
    "Gustavo Earth Quake", "Mariana Water Wave", "Pedro Wind Slash", "Julia Light Beam",
    "Rafael Dark Knight", "Carla Toxic Fang", "Vitor Metal Fist", "Bianca Psychic Storm",
    "Thiago Flame Punch", "Larissa Ice Shield", "Daniel Electric Charge", "Fernanda Poison Mist",
    "Bruno Rock Wall", "Luana Speed Kick", "Henrique Laser Eye", "Paula Ghost Phantom"
]

# Classe Lutador
class Lutador:
    def __init__(self, nome, x, y, cor, esquerda=True):
        self.nome = nome
        self.rect = pygame.Rect(x, y, 50, 100)
        self.velocidade = 5
        self.vida = 100
        self.cor = cor
        self.esquerda = esquerda

    def mover(self, teclas):
        if self.esquerda:
            if teclas[pygame.K_a]:
                self.rect.x -= self.velocidade
            if teclas[pygame.K_d]:
                self.rect.x += self.velocidade
        else:
            if teclas[pygame.K_LEFT]:
                self.rect.x -= self.velocidade
            if teclas[pygame.K_RIGHT]:
                self.rect.x += self.velocidade

    def atacar(self, outro):
        if self.rect.colliderect(outro.rect):
            outro.vida -= 0.3

    def desenhar(self):
        pygame.draw.rect(TELA, self.cor, self.rect)

    def mostrar_vida(self, x, y):
        pygame.draw.rect(TELA, CINZA, (x, y, 200, 20))
        pygame.draw.rect(TELA, VERMELHO, (x, y, 2 * self.vida, 20))
        texto = fonte.render(self.nome, True, BRANCO)
        TELA.blit(texto, (x, y - 25))

# Tela de Seleção de Personagens
def selecao_personagem():
    se
