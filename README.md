# projeto-em-c-procedimento-tdah-asrs18
Projeto feito na linguagem c, que intereja com o usuário fazendo perguntas averiguando se tem a presença do transtorno de défict de atenção e hiperatividade. O procedimento utilizado é o asrs-18, que é um instrumento de autoavaliação para adultos.

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct dados {
  char nome[100];
  char telefone[20];
  char email[100];
  struct dados *prox;
};
// criando struct - dados do usuario 


struct dados *criardados(char *nome, char *telefone, char *email) {
  struct dados *novodados = (struct dados *)malloc(sizeof(struct dados));
  strcpy(novodados->nome, nome);
  strcpy(novodados->telefone, telefone);
  ;
  strcpy(novodados->email, email);
  novodados->prox = NULL;
  return novodados;
}
// Struct dados apontando para função criar dados, criando os dados do usuario

void criarArquivo(struct dados* dados) {
  FILE* arquivo = fopen("dados.txt", "a"); 
  if (arquivo == NULL) {
    printf("Erro ao criar o arquivo.\n");
    return;
  }

    if (dados == NULL) {
      printf("NÃO EXISTEM DADOS.\n");
    } else {
        struct dados* aux = dados;
        while (aux != NULL) {
          fprintf(arquivo, "Nome: %s\n",aux->nome);
          fprintf(arquivo, "Telefone: %s\n",aux->telefone);
          fprintf(arquivo, "E-mail: %s\n",aux->email);
          fprintf(arquivo, "----------------------\n");
          aux = aux->prox;
        }
      printf("Dados escritos no arquivo com sucesso.\n");
      printf("----------------------\n");
    }
    fclose(arquivo); // Fecha o arquivo
}

void exibirdados(struct dados *dados) {
  if (dados == NULL) {
    printf("NÃO EXISTE SEUS DADOS.\n");
  } else {
    struct dados *aux = dados;
    while (aux != NULL) {
      printf("Nome: %s\n", aux->nome);
      printf("Telefone: %s\n", aux->telefone);
      printf("E-mail: %s\n", aux->email);
      aux = aux->prox;
      printf("----------------------------------------------------------\n");
    }
  }
}
// função para exibir os dados do usúario armazenados na lista encadeada

void alerta() {
  printf("----------------------------------------------------------\n");
  printf("⚠ ATENÇÃO: O diagnóstico do Transtorno de Déficit de Atenção e Hiperatividade (TDAH) em crianças, adolescentes e adultos envolve várias etapas. Não existe nenhum teste para TDAH definitivo ou on-line. Estes questionários também não tem validade legal ou para efeito de qualquer tipo de comprovação diagnóstica, servindo apenas como auxiliares à população no intuito de informar e de sugerir uma avaliação médica mais criteriosa.\n");
  printf("----------------------------------------------------------\n");
}
// função alertando que esse procedimento para ser confirmado o transtorno,
// precisa também ser avaliado por um especialista médico

void proc_ques_da() {
  printf("[PROCEDIMENTO DO QUESTIONÁRIO]\n");
  printf("Baseados no questionário autoaplicável ASRS-18, mundialmente reconhecido como instrumento de triagem para sintomas de TDAH em adultos acima de 18 anos. O Resultado do teste indica se há uma necessidade de avaliação médica para melhor investigação diagnóstica.\n");
  printf("----------------------------------------------------------\n");
}
// função explicando sobre o procedimento do questionário.

void resultado() {
  printf("----------------------------------------------------------\n");
  printf("Neste teste é considerado resultado indicativo de avaliação médica "
         "pontuações iguais ou acima de 4.\n");
  printf("----------------------------------------------------------\n");
}
// função resultado para falar sobre o diagnostico positivo sobre o transtorno

int verificarResposta(char *resposta) {
  if (strcmp(resposta, "1") == 0) {
    return 1;
  } else if (strcmp(resposta, "2") == 0) {
    return 0;
  } else {
    printf("Resposta inválida.\n Digite:\n1-SIM ou 2-Não.\n");
    printf("Responda novamente: ");
    scanf("%s", resposta);
    return verificarResposta(resposta);
  }
}
// função recursiva para armazenar a pontuação de acordo com o 1-"sim" respondido pelo usuário.

int main() {
  char resposta[3];
  int opcao = 0;
  int respostas_positivas;
  struct dados *dados = NULL;
  dados = (struct dados *)malloc(sizeof(struct dados));
  printf("----------------------------------------------------------\n");
  printf("<-TESTE DE TDAH->\n");
  printf("----------------------------------------------------------\n");
  
  while (opcao != 3) {
    printf("1 - Adicionar usuário para fazer o teste TDAH.\n");
    printf("2 - Novo usuário para fazer o teste TDAH.\n");
    printf("3 - Sair.\n");
    printf("Escolha uma opção: ");
    scanf("%d", &opcao);
    printf("----------------------------------------------------------\n");

    switch (opcao) {
    case 1:
      printf("Informe seus dados para prosseguir com o teste de TDAH\n");
      printf("----------------------------------------------------------\n");
      printf("Nome: ");
      scanf("%s", dados->nome);
      printf("Telefone: ");
      scanf("%s", dados->telefone);
      printf("E-mail: ");
      scanf("%s", dados->email);
      printf("\n");
      printf("DADOS ARMAZENADOS COM SUCESSO.\n");
      printf("----------------------------------------------------------\n");
      exibirdados(dados);
      struct dados *novodados = criardados(dados->nome, dados->telefone, dados->email);
      criarArquivo(dados);
      int opcao_teste = 0;
      while (opcao_teste != 3){
        printf("1 - Teste: Défict de atenção.\n");
        printf("2 - Teste: Hiperatividade.\n");
        printf("3 - Sair.\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao_teste);
        printf("----------------------------------------------------------\n");

        switch(opcao_teste){
          case 1:
           printf("TESTE [DÉFICIT DE ATENÇÃO]\n");
      // função alerta sobre questionário
           alerta();
           printf("Responda às seguintes perguntas com:\n 1-SIM ou 2-NÃO.\n");
           printf("----------------------------------------------------------\n");
      // função procedimento do questionário
           proc_ques_da();

           respostas_positivas = 0;

           printf("1) Tem dificuldade para lembrar de compromissos ou obrigações? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("2) Tem dificuldade para se concentrar no que as pessoas dizem, mesmo quando elas estão falando diretamente com você? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("3) Tem dificuldade para fazer um trabalho que exige organização? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("4) Você se distrai com atividades ou barulho ao seu redor? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("5) Você deixa um projeto pela metade depois de já ter feito as partes mais difíceis? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("6) Você coloca as coisas fora do lugar ou tem dificuldade em encontrar as coisas em casa ou no trabalho? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("7) Você comete erros por falta de atenção quando tem que trabalhar em um projeto chato ou difícil? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("8) Você tem dificuldade para manter a atenção quando está fazendo um trabalho chato ou repetitivo? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("9) Quando você precisa fazer algo que exige muita concentração, você evita? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);
           printf("----------------------------------------------------------\n");

           resultado();
           printf("RESULTADO:\n");
            
           FILE *arquivo = fopen("dados.txt", "a");
           if (respostas_positivas >= 4) {
             printf("Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             printf("É necessário uma avaliação médica, você apresenta fortes sintomas de déficit de atenção.\n");
             fprintf(arquivo, "Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             fprintf(arquivo, "É necessário uma avaliação médica, você apresenta fortes sintomas de déficit de atenção.\n");
             fprintf(arquivo, "----------------------\n");
           } else {
             printf("Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             printf("Você não apresenta sintomas suficientes de déficit de atenção.\n");
             fprintf(arquivo, "Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             fprintf(arquivo, "Você não apresenta sintomas suficientes de déficit de atenção.\n");
             fprintf(arquivo, "----------------------\n");
           }

          fclose(arquivo);

          break;

        case 2:
          printf("TESTE [HIPERATIVIDADE]\n");
          printf("----------------------------------------------------------\n");
      // função alerta sobre questionário
          alerta();
          printf("Responda às seguintes perguntas com:\n 1- SIM ou 2- NÃO.\n");
          printf("----------------------------------------------------------\n");
      // função procedimento do questionário
          proc_ques_da();

          respostas_positivas = 0;

          printf("1) Você interrompe os outros quando eles estão ocupados? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("2) Você tem dificuldade em esperar nas situações em que cada um tem a sua vez? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("3) Você se sente inquieto(a) ou agitado(a)? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("4) Quando você está conversando, você se pega terminando as frases das pessoas antes delas? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("5) Você se sente ativo(a) demais e necessitando fazer coisas, como se estivesse 'com um motor ligado'? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("6) Você se levanta da cadeira em reuniões ou em outras situações em que deveria ficar sentado(a)? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("7) Você tem dificuldade para sossegar e relaxar quando tem tempo livre para você? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("8) Você fica se mexendo na cadeira ou balançando as mãos ou os pés quando precisa ficar sentado(a) por muito tempo? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("9) Você se pega falando demais em situações sociais? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);
          printf("----------------------------------------------------------\n");

          resultado();
          printf("RESULTADO:\n");
          FILE *arquiv = fopen("dados.txt", "a");
           if (respostas_positivas >= 4) {
             printf("Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             printf("É necessário uma avaliação médica, você apresenta fortes sintomas de hiperatividade.\n");
             fprintf(arquivo, "Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             fprintf(arquivo, "É necessário uma avaliação médica, você apresenta fortes sintomas de hiperatividade.\n");
             fprintf(arquivo, "----------------------\n");
           }else {
             printf("Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             printf("Você não apresenta sintomas suficientes de hiperatividade.\n");
             fprintf(arquivo, "Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             fprintf(arquivo, "Você não apresenta sintomas suficientes de hiperatividade.\n");
             fprintf(arquivo, "----------------------\n");
           }

          fclose(arquivo);

          break;

        case 3:
          printf("Teste de TDAH encerrado :).\n");
          break;

        default:
          printf("Opção inválida, tente novamente.\n");

        }
      }
      break;

    case 2:
      printf("Informe os dados do novo usuário para proseguir com o teste de TDAH.\n");
      printf("----------------------------------------------------------\n");
      printf("Nome: ");
      scanf("%s", dados->nome);
      printf("Telefone: ");
      scanf("%s", dados->telefone);
      printf("E-mail: ");
      scanf("%s", dados->email);
      printf("\n");
      printf("DADOS ARMAZENADOS COM SUCESSO.\n");
      printf("----------------------------------------------------------\n");
      exibirdados(dados);
      struct dados *newdados = criardados(dados->nome, dados->telefone, dados->email);
      criarArquivo(dados);
      int opcao_teste2 = 0;
      while (opcao_teste2 != 3){
        printf("1 - Teste: Défict de atenção.\n");
        printf("2 - Teste: Hiperatividade.\n");
        printf("3 - Sair.\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao_teste2);
        printf("----------------------------------------------------------\n");

        switch(opcao_teste2){
          case 1:
           printf("TESTE [DÉFICIT DE ATENÇÃO]\n");
      // função alerta sobre questionário
           alerta();
           printf("Responda às seguintes perguntas com:\n 1-SIM ou 2-NÃO.\n");
           printf("----------------------------------------------------------\n");
      // função procedimento do questionário
           proc_ques_da();

           respostas_positivas = 0;

           printf("1) Tem dificuldade para lembrar de compromissos ou obrigações? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("2) Tem dificuldade para se concentrar no que as pessoas dizem, mesmo quando elas estão falando diretamente com você? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("3) Tem dificuldade para fazer um trabalho que exige organização? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("4) Você se distrai com atividades ou barulho ao seu redor? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("5) Você deixa um projeto pela metade depois de já ter feito as partes mais difíceis? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("6) Você coloca as coisas fora do lugar ou tem dificuldade em encontrar as coisas em casa ou no trabalho? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("7) Você comete erros por falta de atenção quando tem que trabalhar em um projeto chato ou difícil? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("8) Você tem dificuldade para manter a atenção quando está fazendo um trabalho chato ou repetitivo? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);

           printf("9) Quando você precisa fazer algo que exige muita concentração, você evita? ");
           scanf("%s", resposta);
           respostas_positivas += verificarResposta(resposta);
           printf("----------------------------------------------------------\n");

           resultado();
           printf("RESULTADO:\n");
           FILE *arquivo = fopen("dados.txt", "a");
           if (respostas_positivas >= 4) {
             printf("Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             printf("É necessário uma avaliação médica, você apresenta fortes sintomas de déficit de atenção.\n");
             fprintf(arquivo, "Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             fprintf(arquivo, "É necessário uma avaliação médica, você apresenta fortes sintomas de déficit de atenção.\n");
             fprintf(arquivo, "----------------------\n");
           }else {
             printf("Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             printf("Você não apresenta sintomas suficientes de déficit de atenção.\n");
             fprintf(arquivo, "Pontuação <TESTE DÉFICIT DE ATENÇÃO> = %d.\n", respostas_positivas);
             fprintf(arquivo, "Você não apresenta sintomas suficientes de déficit de atenção.\n");
             fprintf(arquivo, "----------------------\n");
           }

          fclose(arquivo);
          break;

        case 2:
          printf("TESTE [HIPERATIVIDADE]\n");
          printf("----------------------------------------------------------\n");
      // função alerta sobre questionário
          alerta();
          printf("Responda às seguintes perguntas com:\n 1- SIM ou 2- NÃO.\n");
          printf("----------------------------------------------------------\n");
      // função procedimento do questionário
          proc_ques_da();

          respostas_positivas = 0;

          printf("1) Você interrompe os outros quando eles estão ocupados? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("2) Você tem dificuldade em esperar nas situações em que cada um tem a sua vez? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("3) Você se sente inquieto(a) ou agitado(a)? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("4) Quando você está conversando, você se pega terminando as frases das pessoas antes delas? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("5) Você se sente ativo(a) demais e necessitando fazer coisas, como se estivesse 'com um motor ligado'? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("6) Você se levanta da cadeira em reuniões ou em outras situações em que deveria ficar sentado(a)? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("7) Você tem dificuldade para sossegar e relaxar quando tem tempo livre para você? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("8) Você fica se mexendo na cadeira ou balançando as mãos ou os pés quando precisa ficar sentado(a) por muito tempo? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);

          printf("9) Você se pega falando demais em situações sociais? ");
          scanf("%s", resposta);
          respostas_positivas += verificarResposta(resposta);
          printf("----------------------------------------------------------\n");

          resultado();
          printf("RESULTADO:\n");
          FILE *arquiv = fopen("dados.txt", "a");
           if (respostas_positivas >= 4) {
             printf("Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             printf("É necessário uma avaliação médica, você apresenta fortes sintomas de hiperatividade.\n");
             fprintf(arquivo, "Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             fprintf(arquivo, "É necessário uma avaliação médica, você apresenta fortes sintomas de hiperatividade.\n");
             fprintf(arquivo, "----------------------\n");
           }else {
             printf("Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             printf("Você não apresenta sintomas suficientes de hiperatividade.\n");
             fprintf(arquivo, "Pontuação <TESTE HIPERATIVIDADE> = %d.\n", respostas_positivas);
             fprintf(arquivo, "Você não apresenta sintomas suficientes de hiperatividade.\n");
             fprintf(arquivo, "----------------------\n");
           }

          fclose(arquivo);

          break;

        case 3:
          printf("Teste de TDAH encerrado :).\n");
          break;

        default:
          printf("Opção inválida, tente novamente.\n");
  
        }
      } 
      break;

    case 3:
      printf("Questionário encerrado :).\n");
      break;

    default:
      printf("Opção inválida, tente novamente.\n");
    }
  }

  free(dados);
  return 0;
}
