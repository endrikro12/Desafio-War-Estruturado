#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX 20

// ==============================
// STRUCT COMPONENTE
// ==============================
typedef struct {
    char nome[30];
    char tipo[20];
    int prioridade;
} Componente;

// ==============================
// EXIBIÇÃO DE COMPONENTES
// ==============================
void mostrarComponentes(Componente v[], int n) {
    printf("\n=== LISTA DE COMPONENTES ===\n");
    for (int i = 0; i < n; i++) {
        printf("[%d] Nome: %s | Tipo: %s | Prioridade: %d\n",
               i + 1, v[i].nome, v[i].tipo, v[i].prioridade);
    }
    printf("---------------------------\n");
}

// ==============================
// BUBBLE SORT (por nome)
// ==============================
long long bubbleSortNome(Componente v[], int n) {
    long long comparacoes = 0;

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            comparacoes++;
            if (strcmp(v[j].nome, v[j + 1].nome) > 0) {
                Componente aux = v[j];
                v[j] = v[j + 1];
                v[j + 1] = aux;
            }
        }
    }

    return comparacoes;
}

// ==============================
// INSERTION SORT (por tipo)
// ==============================
long long insertionSortTipo(Componente v[], int n) {
    long long comparacoes = 0;

    for (int i = 1; i < n; i++) {
        Componente chave = v[i];
        int j = i - 1;

        while (j >= 0) {
            comparacoes++;
            if (strcmp(chave.tipo, v[j].tipo) < 0) {
                v[j + 1] = v[j];
                j--;
            } else {
                break;
            }
        }

        v[j + 1] = chave;
    }

    return comparacoes;
}

// ==============================
// SELECTION SORT (por prioridade)
// ==============================
long long selectionSortPrioridade(Componente v[], int n) {
    long long comparacoes = 0;

    for (int i = 0; i < n - 1; i++) {
        int min = i;

        for (int j = i + 1; j < n; j++) {
            comparacoes++;
            if (v[j].prioridade < v[min].prioridade) {
                min = j;
            }
        }

        if (min != i) {
            Componente temp = v[i];
            v[i] = v[min];
            v[min] = temp;
        }
    }

    return comparacoes;
}

// ==============================
// BUSCA BINÁRIA POR NOME
// ==============================
int buscaBinariaPorNome(Componente v[], int n, char chave[]) {
    int ini = 0, fim = n - 1;

    while (ini <= fim) {
        int meio = (ini + fim) / 2;
        int cmp = strcmp(chave, v[meio].nome);

        if (cmp == 0)
            return meio;
        else if (cmp > 0)
            ini = meio + 1;
        else
            fim = meio - 1;
    }

    return -1;
}

// ==============================
// FUNÇÃO GENÉRICA PARA MEDIR TEMPO
// ==============================
void medirTempo(const char* nomeAlgoritmo,
                long long (*algoritmo)(Componente[], int),
                Componente v[], int n)
{
    clock_t inicio = clock();
    long long comparacoes = algoritmo(v, n);
    clock_t fim = clock();

    double tempo = (double)(fim - inicio) / CLOCKS_PER_SEC;

    printf("\n=== %s ===\n", nomeAlgoritmo);
    printf("Comparações: %lld\n", comparacoes);
    printf("Tempo: %.6f segundos\n", tempo);
}

// ==============================
// PROGRAMA PRINCIPAL
// ==============================
int main() {
    Componente componentes[MAX];
    int total = 0;
    int opcao;

    int ordenadoPorNome = 0;

    printf("=== SISTEMA DE MONTAGEM DA TORRE ===\n\n");

    printf("Quantos componentes deseja cadastrar (até 20)? ");
    scanf("%d", &total);
    getchar();

    if (total > MAX)
        total = MAX;

    for (int i = 0; i < total; i++) {
        printf("\nCadastro do componente %d:\n", i + 1);

        printf("Nome: ");
        fgets(componentes[i].nome, 30, stdin);
        componentes[i].nome[strcspn(componentes[i].nome, "\n")] = '\0';

        printf("Tipo: ");
        fgets(componentes[i].tipo, 20, stdin);
        componentes[i].tipo[strcspn(componentes[i].tipo, "\n")] = '\0';

        printf("Prioridade (1 a 10): ");
        scanf("%d", &componentes[i].prioridade);
        getchar();
    }

    do {
        printf("\n=== MENU ===\n");
        printf("1 - Ordenar por Nome (Bubble Sort)\n");
        printf("2 - Ordenar por Tipo (Insertion Sort)\n");
        printf("3 - Ordenar por Prioridade (Selection Sort)\n");
        printf("4 - Buscar componente por nome (Busca Binária)\n");
        printf("5 - Mostrar componentes\n");
        printf("0 - Sair\n");
        printf("Escolha: ");

        scanf("%d", &opcao);
        getchar();

        switch (opcao) {

        case 1:
            medirTempo("Bubble Sort (Nome)", bubbleSortNome, componentes, total);
            ordenadoPorNome = 1;
            break;

        case 2:
            medirTempo("Insertion Sort (Tipo)", insertionSortTipo, componentes, total);
            ordenadoPorNome = 0;
            break;

        case 3:
            medirTempo("Selection Sort (Prioridade)", selectionSortPrioridade, componentes, total);
            ordenadoPorNome = 0;
            break;

        case 4:
            if (!ordenadoPorNome) {
                printf("\n❌ A busca binária só funciona após ordenar por NOME.\n");
                break;
            }

            char chave[30];
            printf("Digite o nome do componente a buscar: ");
            fgets(chave, 30, stdin);
            chave[strcspn(chave, "\n")] = '\0';

            int pos = buscaBinariaPorNome(componentes, total, chave);

            if (pos == -1)
                printf("\n❌ Componente não encontrado!\n");
            else
                printf("\n✔ Componente encontrado na posição %d!\n", pos + 1);

            break;

        case 5:
            mostrarComponentes(componentes, total);
            break;

        case 0:
            printf("\nEncerrando...\n");
            break;
        }

    } while (opcao != 0);

    return 0;
}
