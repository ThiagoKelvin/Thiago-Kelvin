#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <locale.h>

void menu();
void cadastro();
void editar();
void consulta();
void pesquisar();
void apagar();
void ajuda();

typedef struct Planta{
	char nome_popular[20];
	char nome_cientifico[20];
	char uso[20];
	char local_de_origem[20];
}Planta;
struct Planta planta[20];

enum Retorno{
	EXITO,
	ERRO_ARQUIVO,
	ERRO_OPCAO
};

void menu(){
	while(1){
	
	system("cls");	
	printf("BEM VINDO AO HERBARIUM DE VANAHEIM\n\n"); 
	unsigned short i;
	printf("\t\tPOR FAVOR, DIGITE:\nCADASTRAR [1]\n\nEDITAR [2]\n\nCONSULTAR [3]\n\nPESQUISAR [4]\n\nAPAGAR [5]\n\nSAIR [6]\n\n\nSE QUISER AJUDA, DIGITE [7]\n\n DÍGITO: ");
	scanf("%i",&i);
	fflush(stdin);

	switch(i){
		case 1: system("cls");
			cadastro();
			break;
		case 2:system("cls");
			editar();
			break;
		case 3:system("cls");
			consulta();
			break;
		case 4: system("cls");
			pesquisar();
			break;
		case 5:	system("cls");
			apagar();
			break;
		case 6:	system("cls");
			printf("OBRIGADO POR UTILIZAR NOSSOS SERVIÇOS!");
			exit(0);
			break;
		case 7: system("cls");
			ajuda();
			break;
		default: ("\nSERVIÇO NÃO DISPONÍVEL OU INEXISTENTE.\n");
		system ("pause");
	}
}}
void cadastro(){

	int count;
	FILE *arquivo;
	arquivo = fopen("arquivos_herbarium.db", "ab");
	if (arquivo == NULL){
    	printf("PROBLEMAS NA ABERTURA DO ARQUIVO\n");
    	return;
		}
	else{

	printf("\nDIGITE OS SEGUINTES DADOS SOBRE A PLANTA:\n");
	fflush(stdin);
	printf("\nNOME POPULAR:\n");
	gets(planta[count].nome_popular);
	fflush(stdin);
	printf("\nNOME CIENTÍFICO:\n");
	gets(planta[count].nome_cientifico);
	fflush(stdin);
	printf("\nUSO:\n");
	gets(planta[count].uso);
	fflush(stdin);
	printf("\nLOCAL DE ORIGEM:\n");
	gets(planta[count].local_de_origem);
	fflush(stdin);
}
fwrite (&planta[count], sizeof (struct Planta), 1, arquivo);
fclose (arquivo);
}

void pesquisar(){

	int tam,count;
	char nome[20];
	FILE*arquivo=fopen("arquivos_herbarium.db","rb");
	if (arquivo == NULL){
    printf("PROBLEMAS NA ABERTURA DO ARQUIVO\n");
	}
		
	fseek(arquivo,0,SEEK_END);
	tam = ftell(arquivo);
	tam = tam / sizeof(struct Planta);
	fseek(arquivo,0,SEEK_SET);
	struct Planta aux[tam];
	for(count=0;count<tam;count++){
		fread(&aux[count],sizeof(struct Planta),1,arquivo);
	}

    printf("\nDIGITE O NOME POPULAR DA PLANTA QUE DESEJA PESQUISAR: ");
    scanf("%s",&nome);

    for(count=0;count<tam;count++){
    if(strcmp(aux[count].nome_popular,nome) == 0){
	printf("NOME POPULAR: %s", planta[count].nome_popular);
	printf("\n\nNOME CIENTÍFICO: %s", planta[count].nome_cientifico);
	printf("\n\nUSO: %s", planta[count].uso);
	printf("\n\nLOCAL DE ORIGEM: %s\n\n", planta[count].local_de_origem);
	getchar();
	}else{
	printf("PLANTYA NÃO DISPONÍVEL OU NÃO ENCONTRADA");
	getchar();
	}
	}
	fclose(arquivo);
	getchar();
}

void editar(){
	int tam,i,j=0,k;
	char nome[30];
	FILE*arquivo=fopen("arquivos_herbarium.db","rb");
	if (arquivo == NULL){
    	printf("PROBLEMAS NA ABERTURA DO ARQUIVO\n");
    }

	fseek(arquivo,0,SEEK_END);
	tam = ftell(arquivo);
	tam = tam / sizeof(struct Planta);
	fseek(arquivo,0,SEEK_SET);

	struct Planta aux[tam];
	struct Planta final[tam];
	for(i=0;i<tam;i++){
		fread(&aux[i],sizeof(struct Planta),1,arquivo);
	}

    printf("\nDIGITE O NOME POPULAR DA PLANTA QUE DESEJA ALTERAR: ");
    scanf("%s",&nome);

    for(i=0;i<tam;i++){
		if(strcmp(aux[i].nome_popular,nome)==0){
        printf("QUAL DADO DESEJA ALTERAR?\n NOME POPULAR[1]\nNOME CIENTÍFICO[2]\nUSO[3]\nLOCAL DE ORIGEM[4]\n\nESCOLHA:%i",k);
        scanf("%i",&k);
        switch(k){
		case 1: fflush(stdin);
				printf("QUAL O NOME POPULAR?\n");
				scanf("%s",&aux[i].nome_popular);
				break;
        case 2: fflush(stdin);
				printf("QUAL O NOME CIENTÍFICO?\n");
				scanf("%s",&aux[i].nome_cientifico);
				break;
		case 3: fflush(stdin);
				printf("QUAL O USO?\n");
				scanf("%s",&aux[i].uso);
				break;
		case 4: fflush(stdin);
				printf("QUAL O LOCAL DE ORIGEM?\n");
				scanf("%s",&aux[i].local_de_origem);
				break;
		}}else{
		final[j]=aux[i];
        }j++;
    }
	for(i=0;i<tam;i++){
	if(strcmp(aux[i].nome_popular,nome)==0){
    	freopen("arquivos_herbarium.db","w+b",arquivo);
    	for(i=0;i<tam;i++){
    	fwrite(&aux[i], sizeof(struct Planta),1,arquivo);
    	}}
    fclose(arquivo);
	printf("EDIÇÃO REALIZADA\n");
    getchar();
	}
}




void consulta(){	int count,tam;
	FILE*arquivo=fopen("arquivos_herbarium.db", "rb");
    fread(&planta[count], sizeof(Planta), 1, arquivo);
	fseek(arquivo,0,SEEK_END);
	tam=ftell(arquivo);
	tam=tam/sizeof(struct Planta);
	struct Planta planta[tam];
	if(arquivo==NULL){
	printf("ARQUIVO INEXISTENTE\n");
    system("pause");
    }
    fseek(arquivo,0,SEEK_SET);
	for(count=0;count<tam;count++){
    fread(&planta[count], sizeof(struct Planta),1,arquivo);
	}
	for(count=0;count<tam;count++){
	printf("NOME POPULAR: %s", planta[count].nome_popular);
	printf("\n\nNOME CIENTÍFICO: %s", planta[count].nome_cientifico);
	printf("\n\nUSO: %s", planta[count].uso);
	printf("\n\nLOCAL DE ORIGEM: %s\n\n", planta[count].local_de_origem);
	}
	fclose(arquivo);
	getchar();
	system("cls");
}


void apagar(){

	int tam,i,j=0;
	char nome[30];
	FILE*arquivo=fopen("arquivos_herbarium.db","rb");
	if (arquivo == NULL){
    printf("PROBLEMAS NA ABERTURA DO ARQUIVO\n");
	}
		
	fseek(arquivo,0,SEEK_END);
	tam = ftell(arquivo);
	tam = tam / sizeof(struct Planta);
	fseek(arquivo,0,SEEK_SET);

	struct Planta aux[tam];
	struct Planta final[tam];
	for(i=0;i<tam;i++){
		fread(&aux[i],sizeof(struct Planta),1,arquivo);
	}

    printf("\nDIGITE O NOME POPULAR DA PLANTA QUE DESEJA EXCLUIR DE NOSSOS ARQUIVOS: ");
    scanf("%s",&nome);

    for(i=0;i<tam;i++){
    if(strcmp(aux[i].nome_popular,nome) == 0){
	}else{
		final[j]=aux[i];
		j++;
	}
    }
    for(i=0;i<tam;i++){
    if(strcmp(aux[i].nome_popular,nome) == 0){
	freopen("arquivos_herbarium.db","w+b",arquivo);
	for(i=0;i<tam-1;i++){
    fwrite(&final[i], sizeof(struct Planta),1,arquivo);
    }
    printf("\n\t\tEXCLUSÃO REALIZADA\n");
	getchar();
}
}
	fclose(arquivo);
	getchar();
}


void ajuda(){
	printf("SE QUISER CADASTRAR ALGUMA ERVA EM NOSSO HERBARIUM, DIGITE 1.\nNO CADASTRO, ESCREVA O NOME POPULAR, NOME CIENTIFICO, O USO E LOCAL DE ORIGEM DA ERVA\nSE QUISER ALTERAR OS DADOS SOBRE ALGUMA PLANTA, DIGITE 2.\nNA EDIÇÃO, DIDITE O NOME POPULAR DA PLANTA E O DADO QUE DESEJA EDITAR\nSE QUISER CONSULTAR NOSSO BANCO DE DADOS, DIGITE 3.\nDIGITE O NOME POPULAR DA ERVA QUE DESEJA SABER OS DADOS\nSE QUISER PESQUISAR ALGUMA PLANTA EM ESPECÍFICO, DIGITE 4.\nSE QUISER APAGAR ALGO DE NOSSO BANCO DE DADOS, DIGITE 5.\nDIGITE O NOME POPULAR DA ERVA QUE DESEJA APAGAR\nSE QUISER SAIR DESTE PROGRAMA, DIGITE 6.\nAPERTE [ENTER] PARA VOLTAR AO MENU APÓS QUALQUER OPERAÇÃO");
getchar();
}
int main (){
setlocale(LC_ALL, "");
menu();
}




