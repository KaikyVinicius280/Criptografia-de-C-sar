# Criptografia-de-C-sar
Projeto em grupo desenvolvido em sala de aula que implementa uma variação da Cifra de César em C. O programa aplica deslocamentos progressivos baseados em diferentes sequências matemáticas (aritmética, geométrica, primos, Fibonacci e incremental), demonstrando lógica, criptografia e manipulação de strings.
# 🔐 Cifra de César Progressiva Avançada

Projeto em grupo desenvolvido em sala de aula que implementa uma variação da **Cifra de César** em **C**.  
O programa aplica deslocamentos progressivos baseados em diferentes sequências matemáticas:  
**Aritmética, Geométrica, Números Primos, Fibonacci** e **Incremental**.  

💡 Objetivo: demonstrar conceitos de **criptografia clássica**, **manipulação de strings** e **lógica de programação em C**.

## 🧠 Funcionalidades
- Escolha entre 5 tipos de progressão de chave  
- Conversão automática para letras maiúsculas  
- Exibição detalhada dos cálculos de deslocamento  
- Limite de até 1000 caracteres por texto  

## ⚙️ Tecnologias Utilizadas
- Linguagem C  
- Bibliotecas: `stdio.h`, `stdlib.h`, `string.h`, `ctype.h`, `math.h`

## 🚀 Execução
Compile e execute o código em um terminal:
```bash
gcc cifra_progressiva.c -o cifra
./cifra

## Código

#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#include <ctype.h>

#include <math.h>



#define MAX_SIZE 1000

#define TAMANHO_ALFABETO 26



const int PRIMOS[] = {

  2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 

  103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 

  211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 

  331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 

  449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577

};

#define MAX_PRIMOS 100

const int FIBONACCI[] = {

  0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 

  6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229

};

#define MAX_FIBONACCI 30

void cifrar_cesar_progressiva(char *texto, int opcao, int chave_inicial, int progressao) {

  if (texto == NULL) {

    return;

  }



  int len = strlen(texto);

  long long deslocamento = 0;



  printf("\n--- Detalhes da Cifra ---\n");

  printf("Chave Inicial: %d\n", chave_inicial);

   

  switch (opcao) {

    case 1:

      printf("Progressão: Aritmética (Razão: %d)\n", progressao);

      break;

    case 2:

      printf("Progressão: Geométrica (Razão: %d)\n", progressao);

      break;

    case 3:

      printf("Progressão: Números Primos\n");

      break;

    case 4:

      printf("Progressão: Série de Fibonacci\n");

      break;

    case 5:

      printf("Progressão: Incremento em Progressão (Razão = Posição)\n");

      break;

  }

  printf("-------------------------\n");



  for (int i = 0; i < len; i++) {

    char caractere_atual = texto[i];



    switch (opcao) {

      case 1:

        deslocamento = chave_inicial + (long long)progressao * i;

        break;

      case 2:

        if (i == 0) {

          deslocamento = chave_inicial + 1;

        } else {

          long long fator_geometrico = 1;

          for (int j = 0; j < i; j++) {

            fator_geometrico = (fator_geometrico * progressao) % (TAMANHO_ALFABETO * 100);

          }

          deslocamento = chave_inicial + fator_geometrico;

        }

        break;

      case 3:

        if (i < MAX_PRIMOS) {

          deslocamento = chave_inicial + PRIMOS[i];

        } else {

          deslocamento = chave_inicial + 0;

        }

        break;

      case 4:

        if (i < MAX_FIBONACCI) {

          deslocamento = chave_inicial + FIBONACCI[i];

        } else {

          deslocamento = chave_inicial + 0;

        }

        break;

      case 5:

        deslocamento = chave_inicial + (long long)i * (i + 1) / 2;

        break;

      default:

        deslocamento = chave_inicial;

        break;

    }



    int shift = (int)(deslocamento % TAMANHO_ALFABETO);



    if (isalpha(caractere_atual)) {

      char base = isupper(caractere_atual) ? 'A' : 'a';

      int indice_original = caractere_atual - base;

      int novo_indice = (indice_original + shift) % TAMANHO_ALFABETO;

      texto[i] = base + novo_indice;

    }

  }

}



int main() {

  char texto_original[MAX_SIZE];

  int opcao;

  int chave_inicial = 2;

  int progressao = 9;



  printf("==========================================\n");

  printf(" Cifra de César Progressiva Avançada em C\n");

  printf("==========================================\n");



  printf("\nEscolha o método de progressão da chave:\n");

  printf("1 - Progressão Aritmética (Razão: %d)\n", progressao);

  printf("2 - Progressão Geométrica (Razão: %d)\n", progressao);

  printf("3 - Números Primos\n");

  printf("4 - Série de Fibonacci\n");

  printf("5 - Incremento em Progressão\n");

  printf("Opção: ");

   

  if (scanf("%d", &opcao) != 1 || opcao < 1 || opcao > 5) {

    printf("\nOpção inválida. Encerrando o programa.\n");

    return 1;

  }

   

  while (getchar() != '\n');



  printf("\nPor favor, digite o texto a ser cifrado (max %d caracteres):\n", MAX_SIZE - 1);

   

  if (fgets(texto_original, MAX_SIZE, stdin) == NULL) {

    fprintf(stderr, "Erro ao ler a entrada.\n");

    return 1;

  }



  texto_original[strcspn(texto_original, "\n")] = 0;



  for (int i = 0; texto_original[i]; i++) {

    texto_original[i] = toupper(texto_original[i]);

  }



  char texto_cifrado[MAX_SIZE];

  strncpy(texto_cifrado, texto_original, MAX_SIZE);

  texto_cifrado[MAX_SIZE - 1] = '\0';



  cifrar_cesar_progressiva(texto_cifrado, opcao, chave_inicial, progressao);



  printf("\n-----------------------------------------\n");

  printf("Texto Original (Maiúsculas): %s\n", texto_original);

  printf("Texto Cifrado:        %s\n", texto_cifrado);

  printf("-----------------------------------------\n");



  return 0;

}
