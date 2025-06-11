#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

char board[8][8];
int currentPlayer = 0; // 0 para Branco, 1 para Preto

void initializeBoard() {
    int r, c;
    char blackPieces[] = {'r', 'n', 'b', 'q', 'k', 'b', 'n', 'r'};
    char whitePieces[] = {'R', 'N', 'B', 'Q', 'K', 'B', 'N', 'R'};

    for (c = 0; c < 8; c++) {
        board[0][c] = blackPieces[c];
        board[1][c] = 'p';
        board[6][c] = 'P';
        board[7][c] = whitePieces[c];
    }
    for (r = 2; r < 6; r++) {
        for (c = 0; c < 8; c++) {
            board[r][c] = ' ';
        }
    }
}

void printBoard() {
    int r, c;
    printf("\n   a b c d e f g h\n");
    printf("  +-----------------+\n");
    for (r = 0; r < 8; r++) {
        printf("%d | ", 8 - r);
        for (c = 0; c < 8; c++) {
            printf("%c ", board[r][c]);
        }
        printf("| %d\n", 8 - r);
    }
    printf("  +-----------------+\n");
    printf("   a b c d e f g h\n\n");
}

int isPathClear(int startRow, int startCol, int endRow, int endCol) {
    int rowStep = (endRow > startRow) ? 1 : ((endRow < startRow) ? -1 : 0);
    int colStep = (endCol > startCol) ? 1 : ((endCol < startCol) ? -1 : 0);
    int currentRow = startRow + rowStep;
    int currentCol = startCol + colStep;

    while (currentRow != endRow || currentCol != endCol) {
        if (board[currentRow][currentCol] != ' ') {
            return 0;
        }
        currentRow += rowStep;
        currentCol += colStep;
    }
    return 1;
}

int isValidMove(int startRow, int startCol, int endRow, int endCol) {
    char piece = board[startRow][startCol];
    char destPiece = board[endRow][endCol];

    if (startRow < 0 || startRow > 7 || startCol < 0 || startCol > 7 ||
        endRow < 0 || endRow > 7 || endCol < 0 || endCol > 7) {
        return 0;
    }

    if (startRow == endRow && startCol == endCol) {
        return 0;
    }

    if ((currentPlayer == 0 && !isupper(piece)) || (currentPlayer == 1 && !islower(piece))) {
        return 0;
    }

    if ((currentPlayer == 0 && isupper(destPiece)) || (currentPlayer == 1 && islower(destPiece))) {
        return 0;
    }

    int rowDiff = abs(endRow - startRow);
    int colDiff = abs(endCol - startCol);

    switch (tolower(piece)) {
        case 'p':
            if (currentPlayer == 0) { // Branco
                if (startCol == endCol && destPiece == ' ') {
                    if (endRow == startRow - 1) return 1;
                    if (startRow == 6 && endRow == startRow - 2 && board[startRow - 1][startCol] == ' ') return 1;
                } else if (colDiff == 1 && endRow == startRow - 1 && islower(destPiece)) {
                    return 1;
                }
            } else { // Preto
                if (startCol == endCol && destPiece == ' ') {
                    if (endRow == startRow + 1) return 1;
                    if (startRow == 1 && endRow == startRow + 2 && board[startRow + 1][startCol] == ' ') return 1;
                } else if (colDiff == 1 && endRow == startRow + 1 && isupper(destPiece)) {
                    return 1;
                }
            }
            break;

        case 'r':
            if ((rowDiff == 0 || colDiff == 0) && isPathClear(startRow, startCol, endRow, endCol)) {
                return 1;
            }
            break;

        case 'n':
            if ((rowDiff == 2 && colDiff == 1) || (rowDiff == 1 && colDiff == 2)) {
                return 1;
            }
            break;

        case 'b':
            if (rowDiff == colDiff && isPathClear(startRow, startCol, endRow, endCol)) {
                return 1;
            }
            break;

        case 'q':
            if (((rowDiff == 0 || colDiff == 0) || (rowDiff == colDiff)) && isPathClear(startRow, startCol, endRow, endCol)) {
                return 1;
            }
            break;

        case 'k':
            if (rowDiff <= 1 && colDiff <= 1) {
                return 1;
            }
            break;
    }

    return 0;
}

int main() {
    char moveStr[5];
    int startRow, startCol, endRow, endCol;

    initializeBoard();

    while (1) {
        printBoard();
        printf("Jogador %s, insira seu movimento (ex: e2e4): ", (currentPlayer == 0) ? "Branco" : "Preto");
        scanf("%4s", moveStr);

        if (strlen(moveStr) != 4 || !isalpha(moveStr[0]) || !isdigit(moveStr[1]) || !isalpha(moveStr[2]) || !isdigit(moveStr[3])) {
            printf("Formato de movimento inválido. Tente novamente.\n");
            while (getchar() != '\n');
            continue;
        }

        startCol = tolower(moveStr[0]) - 'a';
        startRow = '8' - moveStr[1];
        endCol = tolower(moveStr[2]) - 'a';
        endRow = '8' - moveStr[3];

        if (isValidMove(startRow, startCol, endRow, endCol)) {
            board[endRow][endCol] = board[startRow][startCol];
            board[startRow][startCol] = ' ';
            currentPlayer = 1 - currentPlayer;
        } else {
            printf("Movimento inválido! Tente novamente.\n");
        }
         while (getchar() != '\n');
    }

    return 0;
}
