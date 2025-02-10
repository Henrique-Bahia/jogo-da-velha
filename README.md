#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

// Declaração das funções
void inicializarTabuleiro(char tabuleiro[3][3]);
void mostrarTabuleiro(char tabuleiro[3][3]);
bool fazerJogada(char tabuleiro[3][3], int linha, int coluna, char simbolo);
bool verificarVitoria(char tabuleiro[3][3], char simbolo);
bool tabuleiroCheio(char tabuleiro[3][3]);
void jogadaComputador(char tabuleiro[3][3], int dificuldade);
bool jogadaVencedora(char tabuleiro[3][3], int linha, int coluna, char simbolo);
void encontrarMelhorJogada(char tabuleiro[3][3], int& linha, int& coluna);

int main() {
    char tabuleiro[3][3];
    char modoJogo;
    int dificuldade;
    int linha, coluna;
    bool fimDeJogo = false;
    
    srand(time(0));
    inicializarTabuleiro(tabuleiro);
    
    // Perguntar modo de jogo
    cout << "Escolha o modo de jogo:" << endl;
    cout << "1. Jogar com amigo" << endl;
    cout << "2. Jogar com computador" << endl;
    cout << "Digite 1 ou 2: ";
    cin >> modoJogo;
    
    // Se for modo computador, perguntar dificuldade
    if (modoJogo == '2') {
        cout << "\nEscolha o nivel de dificuldade:" << endl;
        cout << "1. Facil " << endl;//(Jogadas aleatorias)
        cout << "2. Medio " << endl;//(Bloqueia algumas jogadas de vitoria)
        cout << "3. Dificil " << endl;//(Joga estrategicamente)
        cout << "Digite 1-3: ";
        cin >> dificuldade;
        
        while (dificuldade < 1 || dificuldade > 3) {
            cout << "Escolha invalida. Digite 1-3: ";
            cin >> dificuldade;
        }
    }
    
    while (!fimDeJogo) {
        // Vez do Jogador 1
        mostrarTabuleiro(tabuleiro);
        cout << "Vez do Jogador 1 (X)" << endl;
        
        do {
            cout << "Digite a linha (1-3): ";
            cin >> linha;
            cout << "Digite a coluna (1-3): ";
            cin >> coluna;
            linha--; coluna--; // Converter para indexação 0-2
        } while (!fazerJogada(tabuleiro, linha, coluna, 'X'));
        
        if (verificarVitoria(tabuleiro, 'X')) {
            mostrarTabuleiro(tabuleiro);
            cout << "Jogador 1 venceu!" << endl;
            break;
        }
        
        if (tabuleiroCheio(tabuleiro)) {
            mostrarTabuleiro(tabuleiro);
            cout << "Empate!" << endl;
            break;
        }
        
        // Vez do Jogador 2 ou Computador
        if (modoJogo == '1') {
            mostrarTabuleiro(tabuleiro);
            cout << "Vez do Jogador 2 (O)" << endl;
            
            do {
                cout << "Digite a linha (1-3): ";
                cin >> linha;
                cout << "Digite a coluna (1-3): ";
                cin >> coluna;
                linha--; coluna--;
            } while (!fazerJogada(tabuleiro, linha, coluna, 'O'));
        } else {
            cout << "Vez do Computador (O)" << endl;
            jogadaComputador(tabuleiro, dificuldade);
        }
        
        if (verificarVitoria(tabuleiro, 'O')) {
            mostrarTabuleiro(tabuleiro);
            if (modoJogo == '1')
                cout << "Jogador 2 venceu!" << endl;
            else
                cout << "Computador venceu!" << endl;
            break;
        }
        
        if (tabuleiroCheio(tabuleiro)) {
            mostrarTabuleiro(tabuleiro);
            cout << "Empate!" << endl;
            break;
        }
    }
    
    return 0;
}

void inicializarTabuleiro(char tabuleiro[3][3]) {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            tabuleiro[i][j] = ' ';
}

void mostrarTabuleiro(char tabuleiro[3][3]) {
    cout << "\n";
    cout << " " << tabuleiro[0][0] << " | " << tabuleiro[0][1] << " | " << tabuleiro[0][2] << endl;
    cout << "---+---+---" << endl;
    cout << " " << tabuleiro[1][0] << " | " << tabuleiro[1][1] << " | " << tabuleiro[1][2] << endl;
    cout << "---+---+---" << endl;
    cout << " " << tabuleiro[2][0] << " | " << tabuleiro[2][1] << " | " << tabuleiro[2][2] << endl;
    cout << "\n";
}

bool fazerJogada(char tabuleiro[3][3], int linha, int coluna, char simbolo) {
    if (linha < 0 || linha > 2 || coluna < 0 || coluna > 2) {
        cout << "Posicao invalida! Linha e coluna devem estar entre 1 e 3." << endl;
        return false;
    }
    
    if (tabuleiro[linha][coluna] != ' ') {
        cout << "Essa posicao ja esta ocupada!" << endl;
        return false;
    }
    
    tabuleiro[linha][coluna] = simbolo;
    return true;
}

bool verificarVitoria(char tabuleiro[3][3], char simbolo) {
    // Verificar linhas e colunas
    for (int i = 0; i < 3; i++) {
        if (tabuleiro[i][0] == simbolo && tabuleiro[i][1] == simbolo && tabuleiro[i][2] == simbolo)
            return true;
        if (tabuleiro[0][i] == simbolo && tabuleiro[1][i] == simbolo && tabuleiro[2][i] == simbolo)
            return true;
    }
    
    // Verificar diagonais
    if (tabuleiro[0][0] == simbolo && tabuleiro[1][1] == simbolo && tabuleiro[2][2] == simbolo)
        return true;
    if (tabuleiro[0][2] == simbolo && tabuleiro[1][1] == simbolo && tabuleiro[2][0] == simbolo)
        return true;
    
    return false;
}

bool tabuleiroCheio(char tabuleiro[3][3]) {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (tabuleiro[i][j] == ' ')
                return false;
    return true;
}

bool jogadaVencedora(char tabuleiro[3][3], int linha, int coluna, char simbolo) {
    char valorOriginal = tabuleiro[linha][coluna];
    tabuleiro[linha][coluna] = simbolo;
    bool vence = verificarVitoria(tabuleiro, simbolo);
    tabuleiro[linha][coluna] = valorOriginal;
    return vence;
}

void encontrarMelhorJogada(char tabuleiro[3][3], int& linha, int& coluna) {
    // Primeiro, tentar vencer
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (tabuleiro[i][j] == ' ') {
                if (jogadaVencedora(tabuleiro, i, j, 'O')) {
                    linha = i;
                    coluna = j;
                    return;
                }
            }
        }
    }
    
    // Segundo, bloquear jogada vencedora do jogador
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (tabuleiro[i][j] == ' ') {
                if (jogadaVencedora(tabuleiro, i, j, 'X')) {
                    linha = i;
                    coluna = j;
                    return;
                }
            }
        }
    }
    
    // Ocupar centro se disponível
    if (tabuleiro[1][1] == ' ') {
        linha = 1;
        coluna = 1;
        return;
    }
    
    // Ocupar cantos se disponíveis
    if (tabuleiro[0][0] == ' ') { linha = 0; coluna = 0; return; }
    if (tabuleiro[0][2] == ' ') { linha = 0; coluna = 2; return; }
    if (tabuleiro[2][0] == ' ') { linha = 2; coluna = 0; return; }
    if (tabuleiro[2][2] == ' ') { linha = 2; coluna = 2; return; }
    
    // Ocupar qualquer borda disponível
    if (tabuleiro[0][1] == ' ') { linha = 0; coluna = 1; return; }
    if (tabuleiro[1][0] == ' ') { linha = 1; coluna = 0; return; }
    if (tabuleiro[1][2] == ' ') { linha = 1; coluna = 2; return; }
    if (tabuleiro[2][1] == ' ') { linha = 2; coluna = 1; return; }
}

void jogadaComputador(char tabuleiro[3][3], int dificuldade) {
    int linha, coluna;
    bool jogadaFeita = false;
    
    switch (dificuldade) {
        case 1: // Fácil - Jogadas aleatórias
            do {
                linha = rand() % 3;
                coluna = rand() % 3;
            } while (tabuleiro[linha][coluna] != ' ');
            break;
            
        case 2: // Médio - Bloqueia jogadas óbvias e faz jogadas vencedoras
            // Primeiro verifica se pode vencer
            for (int i = 0; i < 3 && !jogadaFeita; i++) {
                for (int j = 0; j < 3 && !jogadaFeita; j++) {
                    if (tabuleiro[i][j] == ' ') {
                        if (jogadaVencedora(tabuleiro, i, j, 'O')) {
                            linha = i;
                            coluna = j;
                            jogadaFeita = true;
                        }
                    }
                }
            }
            
            // Depois verifica se precisa bloquear
            if (!jogadaFeita) {
                for (int i = 0; i < 3 && !jogadaFeita; i++) {
                    for (int j = 0; j < 3 && !jogadaFeita; j++) {
                        if (tabuleiro[i][j] == ' ') {
                            if (jogadaVencedora(tabuleiro, i, j, 'X')) {
                                linha = i;
                                coluna = j;
                                jogadaFeita = true;
                            }
                        }
                    }
                }
            }
            
            // Se não houver jogadas vencedoras ou bloqueios necessários, faz jogada aleatória
            if (!jogadaFeita) {
                do {
                    linha = rand() % 3;
                    coluna = rand() % 3;
                } while (tabuleiro[linha][coluna] != ' ');
            }
            break;
            
        case 3: // Difícil - Jogo estratégico
            encontrarMelhorJogada(tabuleiro, linha, coluna);
            break;
    }
    
    tabuleiro[linha][coluna] = 'O';
}
