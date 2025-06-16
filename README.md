#include <stdio.h>

int main() {
    char tabuleiro[8][8];
    int i, j;
    int cavalo_x = 7;
    int cavalo_y = 1;
    int peao_x = 5;
    int peao_y = 2;
    int mov_x, mov_y;
    int delta_x, delta_y;

    for (i = 0; i < 8; i++) {
        for (j = 0; j < 8; j++) {
            tabuleiro[i][j] = '.';
        }
    }

    tabuleiro[cavalo_x][cavalo_y] = 'C';
    tabuleiro[peao_x][peao_y] = 'P';

    printf("   a b c d e f g h\n");
    printf("  -----------------\n");
    for (i = 0; i < 8; i++) {
        printf("%d| ", 8 - i);
        for (j = 0; j < 8; j++) {
            printf("%c ", tabuleiro[i][j]);
        }
        printf("|\n");
    }
    printf("  -----------------\n");

    printf("\nDesafio do Cavalo!\n");
    printf("Capture o Peao (P) com o seu Cavalo (C).\n");
    
    printf("\nDigite a linha de destino (1-8): ");
    scanf("%d", &mov_x);
    printf("Digite a coluna de destino (1-8, onde a=1, b=2...): ");
    scanf("%d", &mov_y);

    mov_x = 8 - mov_x;
    mov_y = mov_y - 1;

    delta_x = mov_x - cavalo_x;
    if (delta_x < 0) {
        delta_x = -delta_x;
    }

    delta_y = mov_y - cavalo_y;
    if (delta_y < 0) {
        delta_y = -delta_y;
    }

    if ((delta_x == 1 && delta_y == 2) || (delta_x == 2 && delta_y == 1)) {
        if (mov_x == peao_x && mov_y == peao_y) {
            printf("\nBOA! Voce capturou o peao!\n");
        } else {
            printf("\nQuase! Movimento valido, mas nao capturou a peca.\n");
        }
    } else {
        printf("\nOps! Esse nao e um movimento valido para o cavalo.\n");
    }

    return 0;
}
