import pygame
import sys

pygame.init()

# Configuração da tela
LARGURA, ALTURA = 900, 600
TELA = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("🔥 Fogo de Luta Livre 🔥")

# Cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
CINZA = (100, 100, 100)
AMARELO = (255, 255, 0)

# Outras cores para os personagens
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

def selecao_personagem():
    selecionados = []
    while len(selecionados) < 2:
        TELA.fill(PRETO)
        titulo = fonte.render("Escolha 2 personagens (Teclas 1 a 0, depois Q até P)", True, BRANCO)
        TELA.blit(titulo, (150, 20))

        for i, nome in enumerate(personagens):
            cor = CORES[i % len(CORES)]
            texto = fonte.render(f"{i+1} - {nome}", True, cor)
            TELA.blit(texto, (100, 60 + i * 25))

        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                escolha = -1
                if pygame.K_1 <= evento.key <= pygame.K_9:
                    escolha = evento.key - pygame.K_1
                elif evento.key == pygame.K_0:
                    escolha = 9
                elif evento.key == pygame.K_q:
                    escolha = 10
                elif evento.key == pygame.K_w:
                    escolha = 11
                elif evento.key == pygame.K_e:
                    escolha = 12
                elif evento.key == pygame.K_r:
                    escolha = 13
                elif evento.key == pygame.K_t:
                    escolha = 14
                elif evento.key == pygame.K_y:
                    escolha = 15
                elif evento.key == pygame.K_u:
                    escolha = 16
                elif evento.key == pygame.K_i:
                    escolha = 17
                elif evento.key == pygame.K_o:
                    escolha = 18
                elif evento.key == pygame.K_p:
                    escolha = 19

                if 0 <= escolha < len(personagens) and escolha not in selecionados:
                    selecionados.append(escolha)
    return selecionados

def fim_de_jogo(vencedor):
    while True:
        TELA.fill(PRETO)
        texto = fonte.render(f"{vencedor} VENCEU!", True, AMARELO)
        continuar = fonte.render("Pressione ENTER para voltar ao menu", True, BRANCO)
        TELA.blit(texto, (300, 250))
        TELA.blit(continuar, (220, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN and evento.key == pygame.K_RETURN:
                main()

def jogo(personagem1_idx, personagem2_idx):
    lutador1 = Lutador(personagens[personagem1_idx], 100, 400, CORES[personagem1_idx], True)
    lutador2 = Lutador(personagens[personagem2_idx], 700, 400, CORES[personagem2_idx], False)

    while True:
        clock.tick(FPS)
        TELA.fill(PRETO)

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        teclas = pygame.key.get_pressed()
        lutador1.mover(teclas)
        lutador2.mover(teclas)

        # Ataque simples
        if teclas[pygame.K_SPACE]:
            lutador1.atacar(lutador2)
        if teclas[pygame.K_RETURN]:
            lutador2.atacar(lutador1)

        # Fim de jogo
        if lutador1.vida <= 0 or lutador2.vida <= 0:
            vencedor = lutador1.nome if lutador1.vida > 0 else lutador2.nome
            fim_de_jogo(vencedor)
            return

        # Desenhar jogadores e vida
        lutador1.desenhar()
        lutador2.desenhar()
        lutador1.mostrar_vida(50, 50)
        lutador2.mostrar_vida(650, 50)

        pygame.display.flip()

def main():
    while True:
        TELA.fill(PRETO)
        titulo = fonte.render("🔥 Fogo de Luta Livre 🔥", True, VERMELHO)
        start = fonte.render("Pressione ENTER para começar", True, BRANCO)
        TELA.blit(titulo, (300, 200))
        TELA.blit(start, (250, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_RETURN:
                    selecionados = selecao_personagem()
                    jogo(selecionados[0], selecionados[1])

main()
import pygame
import sys
import random

pygame.init()

# Configurações da tela
LARGURA, ALTURA = 900, 600
TELA = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("🔥 Fogo de Luta Livre 🔥")

# Cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
VERDE = (0, 255, 0)
AZUL = (0, 0, 255)
AMARELO = (255, 255, 0)
ROXO = (150, 0, 255)
CINZA = (100, 100, 100)
CORES = [VERMELHO, AZUL, VERDE, AMARELO, ROXO, BRANCO, (255,128,0), (128,0,255), (255,50,50), (50,50,255),
         (50,255,50), (200,200,50), (200,50,200), (50,200,200), (150,75,0), (75,0,150), (100,100,100), 
         (255,100,180), (180,255,100), (100,180,255)]

# Fonte
fonte = pygame.font.SysFont("Arial", 28)

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
        self.vel = 5
        self.vida = 100
        self.cor = cor
        self.esquerda = esquerda

    def mover(self, teclas):
        if self.esquerda:
            if teclas[pygame.K_a]:
                self.rect.x -= self.vel
            if teclas[pygame.K_d]:
                self.rect.x += self.vel
        else:
            if teclas[pygame.K_LEFT]:
                self.rect.x -= self.vel
            if teclas[pygame.K_RIGHT]:
                self.rect.x += self.vel

    def atacar(self, outro):
        if self.rect.colliderect(outro.rect):
            outro.vida -= 0.3

    def desenhar(self):
        pygame.draw.rect(TELA, self.cor, self.rect)

    def barra_vida(self, x, y):
        pygame.draw.rect(TELA, CINZA, (x, y, 200, 20))
        pygame.draw.rect(TELA, VERMELHO, (x, y, 2 * self.vida, 20))
        texto = fonte.render(self.nome, True, BRANCO)
        TELA.blit(texto, (x, y - 25))

# Tela de seleção de personagens
def selecao_personagem():
    selecionados = []
    while len(selecionados) < 2:
        TELA.fill(PRETO)
        titulo = fonte.render("Escolha 2 Personagens (1 a 20)", True, BRANCO)
        TELA.blit(titulo, (250, 30))

        for i, nome in enumerate(personagens):
            texto = fonte.render(f"{i+1} - {nome}", True, CORES[i])
            TELA.blit(texto, (100, 70 + i * 25))

        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                if pygame.K_1 <= evento.key <= pygame.K_9:
                    escolha = evento.key - pygame.K_1
                elif pygame.K_KP1 <= evento.key <= pygame.K_KP9:
                    escolha = evento.key - pygame.K_KP1
                elif evento.key == pygame.K_0:
                    escolha = 9
                elif evento.key == pygame.K_q:
                    escolha = 10
                elif evento.key == pygame.K_w:
                    escolha = 11
                elif evento.key == pygame.K_e:
                    escolha = 12
                elif evento.key == pygame.K_r:
                    escolha = 13
                elif evento.key == pygame.K_t:
                    escolha = 14
                elif evento.key == pygame.K_y:
                    escolha = 15
                elif evento.key == pygame.K_u:
                    escolha = 16
                elif evento.key == pygame.K_i:
                    escolha = 17
                elif evento.key == pygame.K_o:
                    escolha = 18
                elif evento.key == pygame.K_p:
                    escolha = 19
                else:
                    continue

                if 0 <= escolha < len(personagens):
                    if personagens[escolha] not in selecionados:
                        selecionados.append(escolha)
    return selecionados

# Função principal do jogo
def jogo(personagem1_idx, personagem2_idx):
    lutador1 = Lutador(personagens[personagem1_idx], 100, 400, CORES[personagem1_idx], True)
    lutador2 = Lutador(personagens[personagem2_idx], 700, 400, CORES[personagem2_idx], False)

    while True:
        clock.tick(FPS)
        TELA.fill(PRETO)

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        teclas = pygame.key.get_pressed()
        lutador1.mover(teclas)
        lutador2.mover(teclas)

        # Ataque simples
        if teclas[pygame.K_SPACE]:
            lutador1.atacar(lutador2)
        if teclas[pygame.K_RETURN]:
            lutador2.atacar(lutador1)

        # Verificar fim de jogo
        if lutador1.vida <= 0 or lutador2.vida <= 0:
            vencedor = lutador1.nome if lutador1.vida > 0 else lutador2.nome
            fim_de_jogo(vencedor)
            return

        # Desenhar
        lutador1.desenhar()
        lutador2.desenhar()
        lutador1.barra_vida(50, 50)
        lutador2.barra_vida(650, 50)

        pygame.display.flip()

# Tela de vitória
def fim_de_jogo(vencedor):
    while True:
        TELA.fill(PRETO)
        texto = fonte.render(f"{vencedor} VENCEU!", True, AMARELO)
        continuar = fonte.render("Pressione ENTER para voltar ao menu", True, BRANCO)
        TELA.blit(texto, (300, 250))
        TELA.blit(continuar, (220, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN and evento.key == pygame.K_RETURN:
                main()

# Menu principal
def main():
    while True:
        TELA.fill(PRETO)
        titulo = fonte.render("🔥 Fogo de Luta Livre 🔥", True, VERMELHO)
        start = fonte.render("Pressione ENTER para Começar", True, BRANCO)
        TELA.blit(titulo, (300, 200))
        TELA.blit(start, (250, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_RETURN:
                    selecionados = selecao_personagem()
                    jogo(selecionados[0], selecionados[1])

main()
import pygame
import sys
import random

pygame.init()

# Configurações da tela
LARGURA, ALTURA = 900, 600
TELA = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("🔥 Fogo de Luta Livre 🔥")

# Cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
VERDE = (0, 255, 0)
AZUL = (0, 0, 255)
AMARELO = (255, 255, 0)
ROXO = (150, 0, 255)
CINZA = (100, 100, 100)
CORES = [VERMELHO, AZUL, VERDE, AMARELO, ROXO, BRANCO, (255,128,0), (128,0,255), (255,50,50), (50,50,255),
         (50,255,50), (200,200,50), (200,50,200), (50,200,200), (150,75,0), (75,0,150), (100,100,100), 
         (255,100,180), (180,255,100), (100,180,255)]

# Fonte
fonte = pygame.font.SysFont("Arial", 28)

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
        self.vel = 5
        self.vida = 100
        self.cor = cor
        self.esquerda = esquerda

    def mover(self, teclas):
        if self.esquerda:
            if teclas[pygame.K_a]:
                self.rect.x -= self.vel
            if teclas[pygame.K_d]:
                self.rect.x += self.vel
        else:
            if teclas[pygame.K_LEFT]:
                self.rect.x -= self.vel
            if teclas[pygame.K_RIGHT]:
                self.rect.x += self.vel

    def atacar(self, outro):
        if self.rect.colliderect(outro.rect):
            outro.vida -= 0.3

    def desenhar(self):
        pygame.draw.rect(TELA, self.cor, self.rect)

    def barra_vida(self, x, y):
        pygame.draw.rect(TELA, CINZA, (x, y, 200, 20))
        pygame.draw.rect(TELA, VERMELHO, (x, y, 2 * self.vida, 20))
        texto = fonte.render(self.nome, True, BRANCO)
        TELA.blit(texto, (x, y - 25))

# Tela de seleção de personagens
def selecao_personagem():
    selecionados = []
    while len(selecionados) < 2:
        TELA.fill(PRETO)
        titulo = fonte.render("Escolha 2 Personagens (1 a 20)", True, BRANCO)
        TELA.blit(titulo, (250, 30))

        for i, nome in enumerate(personagens):
            texto = fonte.render(f"{i+1} - {nome}", True, CORES[i])
            TELA.blit(texto, (100, 70 + i * 25))

        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                if pygame.K_1 <= evento.key <= pygame.K_9:
                    escolha = evento.key - pygame.K_1
                elif pygame.K_KP1 <= evento.key <= pygame.K_KP9:
                    escolha = evento.key - pygame.K_KP1
                elif evento.key == pygame.K_0:
                    escolha = 9
                elif evento.key == pygame.K_q:
                    escolha = 10
                elif evento.key == pygame.K_w:
                    escolha = 11
                elif evento.key == pygame.K_e:
                    escolha = 12
                elif evento.key == pygame.K_r:
                    escolha = 13
                elif evento.key == pygame.K_t:
                    escolha = 14
                elif evento.key == pygame.K_y:
                    escolha = 15
                elif evento.key == pygame.K_u:
                    escolha = 16
                elif evento.key == pygame.K_i:
                    escolha = 17
                elif evento.key == pygame.K_o:
                    escolha = 18
                elif evento.key == pygame.K_p:
                    escolha = 19
                else:
                    continue

                if 0 <= escolha < len(personagens):
                    if personagens[escolha] not in selecionados:
                        selecionados.append(escolha)
    return selecionados

# Função principal do jogo
def jogo(personagem1_idx, personagem2_idx):
    lutador1 = Lutador(personagens[personagem1_idx], 100, 400, CORES[personagem1_idx], True)
    lutador2 = Lutador(personagens[personagem2_idx], 700, 400, CORES[personagem2_idx], False)

    while True:
        clock.tick(FPS)
        TELA.fill(PRETO)

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        teclas = pygame.key.get_pressed()
        lutador1.mover(teclas)
        lutador2.mover(teclas)

        # Ataque simples
        if teclas[pygame.K_SPACE]:
            lutador1.atacar(lutador2)
        if teclas[pygame.K_RETURN]:
            lutador2.atacar(lutador1)

        # Verificar fim de jogo
        if lutador1.vida <= 0 or lutador2.vida <= 0:
            vencedor = lutador1.nome if lutador1.vida > 0 else lutador2.nome
            fim_de_jogo(vencedor)
            return

        # Desenhar
        lutador1.desenhar()
        lutador2.desenhar()
        lutador1.barra_vida(50, 50)
        lutador2.barra_vida(650, 50)

        pygame.display.flip()

# Tela de vitória
def fim_de_jogo(vencedor):
    while True:
        TELA.fill(PRETO)
        texto = fonte.render(f"{vencedor} VENCEU!", True, AMARELO)
        continuar = fonte.render("Pressione ENTER para voltar ao menu", True, BRANCO)
        TELA.blit(texto, (300, 250))
        TELA.blit(continuar, (220, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN and evento.key == pygame.K_RETURN:
                main()

# Menu principal
def main():
    while True:
        TELA.fill(PRETO)
        titulo = fonte.render("🔥 Fogo de Luta Livre 🔥", True, VERMELHO)
        start = fonte.render("Pressione ENTER para Começar", True, BRANCO)
        TELA.blit(titulo, (300, 200))
        TELA.blit(start, (250, 300))
        pygame.display.flip()

        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_RETURN:
                    selecionados = selecao_personagem()
                    jogo(selecionados[0], selecionados[1])

main()
