#include <stdio.h>
#include <string.h>

/*
    ------------------------------------------
        DESAFIO WAR ESTRUTURADO - NÍVEL NOVATO
    ------------------------------------------

    Objetivo:
    Criar um sistema que cadastre 5 territórios com:
        - Nome do território
        - Cor do exército
        - Número de tropas

    Após o cadastro, o sistema exibe todos os dados cadastrados.

    Conceitos abordados:
        - struct (estrutura de dados composta)
        - vetor estático
        - entrada e saída de dados (scanf, fgets, printf)
*/

/* 
   Definição da estrutura (struct) que representa um território.
   Cada território contém:
       - nome: nome do território
       - cor: cor do exército dominante
       - tropas: quantidade de tropas no território
*/
struct Territorio {
    char nome[30];
    char cor[10];
    int tropas;
};

int main() {
    // Declaração de um vetor com 5 elementos do tipo Territorio
    struct Territorio territorios[5];

    printf("=== CADASTRO DE TERRITORIOS ===\n\n");

    // Laço para cadastro dos 5 territórios
    for (int i = 0; i < 5; i++) {
        printf("Cadastro do Territorio %d\n", i + 1);

        // Lendo o nome do território
        printf("Digite o nome do territorio: ");
        fgets(territorios[i].nome, sizeof(territorios[i].nome), stdin);
        // Remove o '\n' do final da string
        territorios[i].nome[strcspn(territorios[i].nome, "\n")] = '\0';

        // Lendo a cor do exército
        printf("Digite a cor do exercito: ");
        fgets(territorios[i].cor, sizeof(territorios[i].cor), stdin);
        territorios[i].cor[strcspn(territorios[i].cor, "\n")] = '\0';

        // Lendo o número de tropas
        printf("Digite a quantidade de tropas: ");
        scanf("%d", &territorios[i].tropas);

        // Limpa o buffer do teclado antes da próxima leitura de string
        getchar();

        printf("\n");
    }

    // Exibição dos dados cadastrados
    printf("\n=== ESTADO ATUAL DO MAPA ===\n\n");
    for (int i = 0; i < 5; i++) {
        printf("Territorio %d:\n", i + 1);
        printf("Nome: %s\n", territorios[i].nome);
        printf("Cor do Exercito: %s\n", territorios[i].cor);
        printf("Tropas: %d\n", territorios[i].tropas);
        printf("-------------------------------\n");
    }

    printf("\nCadastro concluido com sucesso!\n");

    return 0;
}
