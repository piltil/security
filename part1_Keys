//keys

#include "stdio.h"
#include "stdlib.h"
#include "string.h"

int main (int argc, char **argv )
{
	
	int i = 0, j = 0, c = 0;
	size_t bufferSize = 1024;
	char  *buffer = (char *)calloc(1024, sizeof(char)); //big enough to read one line
	const int N = 100;
	char **strips = NULL;
	int globalOffset = 0;
	int **offsetForElement = NULL;
	int *offsetCnt = (int *)calloc(N, sizeof(int ));
	int **potentialKeysId = NULL;
	int *potentialKeysCnt = (int *)calloc(N, sizeof(int ));
	int *offsetFoundTimes = (int *)calloc(N, sizeof(int ));
	int stripId = 0;
	char plaintext[]  = "TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITY";
	char ciphertext[] = "BBQGFHSDXNFKLXRREYYPADREWTFRJGJDCBGDZFXINXMWYLHT";
	int index_plaintext = -1, index_ciphertext = -1;
	int index_p2 = -1, 			index_c2 = -1;
	int d1 = -1, d2 = -1, d11 = -1, d22 = -1;
	int stripsNo = 0,  elementNo = 0;
	int thisNum = 0;
	int findPrevious = 0;
	int isDecrypt = 1;	//if true, decrypt an input string; otherwse, encrypt input string
	char inputStr[1025];
	int stripIndeces[] =     {46, 59, 9, 13,   8,  90, 30,  34,  62, 83, 51, 55, 49, 88, 27, 92, 69, 40, 74, 17, 21, 71, 45, 79, 96 };
	//array to hold the targeted char, like 'V' (for strips[90]), 'S' (for strips[8])
		
	char  targetedChars[] = {'S','Q','R','X', 'S', 'V','N', 'M','W','Z','P','Z','X','S','X','U','P','S','X','J', 'I','Y','B','Y','A'};
	//char targetedChars = "SQRXSVNMWZPZXSXUPSXJIYBYA";
	//array to hold all result index, coresponding to above stripsIndex, like [5] (for strips[90], [4] (for strips[4])
	int resultIndeces[] =   { 0,  1,  2,  3,  4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24};
	int result[26];
	int indeces[26];
	int stripIndex = 0;
	char targetChar = 'A';
	int resultIndex = 0;

	//read in inputs
	//step 1: readin strips, some of which will be used as keys
	FILE *fp = fopen("additional.txt", "r");
	if ( NULL == fp ) {
		printf("can not find file additional.txt to read. \n");
		//exit(-1);
	}

	strips = (char **)calloc(N, sizeof(char *));
	for(i=0; i<N; i++) {
		strips[i] = (char *)calloc(53, sizeof(char));
	}
	//step 2: find the offset between plain- and ciphertext
	// 			by checking the one that appears in every of 23 strips (plain- and ciper-text)
	while (-1 != getline(&buffer, &bufferSize, fp) ) {
		for (i=0; i<52; i++ ){
			strips[stripId][i] = buffer[i];
		}
		stripId ++;
	}
	fclose(fp);
	if (stripId != 100) {
		printf("Warning! not 100 strips was read. May be wrong. \n");
	}

		
	for(globalOffset=1; globalOffset < 25; globalOffset ++) {	
		printf("With offset %d, plain text is ", globalOffset);
		for (i=0; i<25; i ++) {
			stripIndex = stripIndeces[i];
			targetChar = targetedChars[i];
			resultIndex = resultIndeces[i];
		
			for(j=0; j<52; j++) {
				if (strips[stripIndex][j] == targetChar ) {
					indeces[resultIndex]  = j + 26;
					result[resultIndex] = strips[stripIndex][j+26-globalOffset];
					break;
				}
			}
		}
		for (i=0; i<25; i ++) {
			printf("%c", result[i]);
		}
		printf("\n");
	}

	for(i=0; i<N; i++) {
		free(strips[i]);
	}
	free(strips);

	return 0;
}

