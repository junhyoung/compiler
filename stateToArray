#include<stdio.h>
#include<stdlib.h>
#include"st_tab.h"

typedef struct match
{
	int index;
	char terminal;
}match;
void put_table(int tab[][SYMBOLS], int m, int n)
{
	int i;

	puts("State Transition Table");
	puts("\t0\t1");
	for (i = 0; i < m; i++)
		printf("%c\t%c\t%c\n",
			'A' + i, 'A' + tab[i][0], 'A' + tab[i][1]);
	puts("Final states = {} ");
}
void main() {
	FILE *file= fopen("input.txt", "r");
	FILE* out = fopen("st_tab.h", "w");
	match* m;
	int i,j,k;
	int States = 1; //nonTerminal
	int Symbols = 1; //terminal
	int Final = 0;
	char *fin;
	char temp[100];
	int **arr;
	char t;
	int isTrue = 0;
	int tp;
	int ch;	/* input symbol */
	int cur_state = 0;	/* start state */

	while (1) { //주석 걸러내는 작업
		fscanf(file, "%c", &t);
		if (t != ';') {
			fscanf(file, "%s", temp);
			break;
		}
		else {
			while (1) {
				fscanf(file, "%c", &t);
				if (t == '\n')
					break;
			}
		}
	}

	for (i = 0; temp[i]; i++) { //Terminals=~~
		if (isTrue)
			Symbols++;
		if (temp[i] == '=')
			isTrue = 1;
	}

	fscanf(file, "%s", temp); //Nonterminals=~~
	isTrue = 0;

	for (i = 0; temp[i]; i++) {
		if (isTrue)
			States++;
		if (temp[i] == '=')
			isTrue = 1;
	}


	m = (match*)malloc(sizeof(match)*States);
	arr = (int*)malloc(sizeof(int*)*(States));
	for (int i = 0; i < States; i++)
		arr[i] = (int)malloc(sizeof(int)*(Symbols));
	isTrue = 0;
	for (i = 0; temp[i]; i++) {
		if (isTrue) {
			m[i - tp].index = i - tp;
			m[i - tp].terminal = temp[i];
		}
		if (temp[i] == '=') {
			tp = i + 1;
			isTrue = 1;
		}
	}

	isTrue = 0;
	while (1) { //Final State~~
		fscanf(file, "%c", &t);
		if (t == '=') {
			fscanf(file, "%s", temp);
			break;
		}
	}
	for (int i = 0; temp[i]; i++) 
		Final++;
	fin = (char*)malloc(sizeof(char)*Final);
	for (int i = 0; i < Final; i++) {
		for (j = 0; j < States; j++) {
			if (temp[i] == m[j].terminal) {
				fin[i] = m[j].index;
				break;
			}
 		}
	}

	isTrue = 0;
	
	fscanf(file, "%c", &t); //'\0'제거

	//while (1) {//Final states=~~
	//	fscanf(file, "%c", &t); 
	//	if (t == '\n')
	//		break;
	//}
	

	for (i = 0; i < States; i++) {
		for (j = 0; j < Symbols; j++) {
			while (1) {
				fscanf(file, "%c", &t);
				if (t != ' ' && t != '\n')
					break;
			}
			for (k = 0; k < States; k++) {
				if (m[k].terminal == t) {
					arr[i][j] = m[k].index;
					break;
				}

			}
			if(t=='_')
				arr[i][j] = -1;
		}
	}



	
	
	fprintf(out, "#define STATES %d\n", States-1);
	fprintf(out, "#define SYMBOLS %d\n", Symbols-1);
	fprintf(out, "int ST_TAB[STATES][SYMBOLS] = {\n");

	for (i = 0; i < States-1; i++) {
		fprintf(out, "{");
		for (j = 0; j < Symbols; j++) {
			if(j==Symbols-1)
				fprintf(out, "%d",arr[i][j]);
			else if(j>=1)
				fprintf(out, "%d,",arr[i][j]);
		}
		if(i==States-1)
			fprintf(out, "}\n");
		else
			fprintf(out, "},\n");
	}
	fprintf(out, "};\n");
	fclose(file);
	fclose(out);
	


	put_table(ST_TAB, STATES, SYMBOLS);

	ch = getchar();
	while (ch != '\n') {
		cur_state = ST_TAB[cur_state][ch - '0'];
		if (cur_state < 0) { puts("FAIL"); return; }
		ch = getchar();
	}
	isTrue = 0;
	for (int i = 0; i < Final; i++) {
		if (cur_state == fin[i]) {
			isTrue = 1;
			break;
		}
		
	}
	if(isTrue)
		puts("OK");   /* final states = { 2 } */
	else puts("FAIL");
}
