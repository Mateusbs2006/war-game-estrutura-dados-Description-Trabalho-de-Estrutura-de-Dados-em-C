# war-game-estrutura-dados-Description-Trabalho-de-Estrutura-de-Dados-em-C#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

typedef struct {
    char nome[30];
    char cor[10];
    int tropas;
} Territorio;

void cadastrarTerritorios(Territorio* mapa, int n);
void exibirMapa(Territorio* mapa, int n);
void atacar(Territorio* atacante, Territorio* defensor);
void atribuirMissao(char** destino, char* listaMissoes[], int total);
void liberarMemoria(Territorio* mapa, char* missao);

int main() {
    srand(time(NULL));
    int numTerritorios;

    printf("--- WAR ESTRUTURADO ---\n");
    printf("Quantos territorios deseja cadastrar? ");
    scanf("%d", &numTerritorios);

    Territorio* mapa = (Territorio*)calloc(numTerritorios, sizeof(Territorio));
    if (mapa == NULL) return 1;

    cadastrarTerritorios(mapa, numTerritorios);
    exibirMapa(mapa, numTerritorios);

    char* listaMissoes[] = {
        "Conquistar 3 territorios",
        "Eliminar a cor azul",
        "Dominar a America do Sul",
        "Conquistar a Asia",
        "Eliminar todas as tropas vermelhas"
    };
    char* missaoJogador = (char*)malloc(100 * sizeof(char));
    atribuirMissao(&missaoJogador, listaMissoes, 5);
    printf("\nSua missao: %s\n", missaoJogador);

    if (numTerritorios >= 2) {
        printf("\nSimulando ataque entre o primeiro e o segundo territorio...\n");
        atacar(&mapa[0], &mapa[1]);
        exibirMapa(mapa, numTerritorios);
    }

    liberarMemoria(mapa, missaoJogador);
    return 0;
}

void cadastrarTerritorios(Territorio* mapa, int n) {
    for (int i = 0; i < n; i++) {
        printf("\nTerritorio %d:\n", i + 1);
        printf("Nome: "); scanf("%s", mapa[i].nome);
        printf("Cor: "); scanf("%s", mapa[i].cor);
        printf("Tropas: "); scanf("%d", &mapa[i].tropas);
    }
}

void exibirMapa(Territorio* mapa, int n) {
    printf("\n--- ESTADO DO MAPA ---\n");
    for (int i = 0; i < n; i++) {
        printf("[%s] Cor: %s | Tropas: %d\n", mapa[i].nome, mapa[i].cor, mapa[i].tropas);
    }
}

void atacar(Territorio* atacante, Territorio* defensor) {
    int dadoAtacante = (rand() % 6) + 1;
    int dadoDefensor = (rand() % 6) + 1;

    printf("Atacante (%s) tirou: %d\n", atacante->nome, dadoAtacante);
    printf("Defensor (%s) tirou: %d\n", defensor->nome, dadoDefensor);

    if (dadoAtacante > dadoDefensor) {
        printf("Vitoria do atacante! O territorio mudou de dono.\n");
        strcpy(defensor->cor, atacante->cor);
        defensor->tropas = atacante->tropas / 2;
    } else {
        printf("Defesa resistiu! Atacante perdeu 1 tropa.\n");
        atacante->tropas--;
    }
}

void atribuirMissao(char** destino, char* listaMissoes[], int total) {
    int sorteio = rand() % total;
    strcpy(*destino, listaMissoes[sorteio]);
}

void liberarMemoria(Territorio* mapa, char* missao) {
    free(mapa);
    free(missao);
}
