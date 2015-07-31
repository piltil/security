//keys

#include "stdio.h"
#include "stdlib.h"
#include "string.h"

// add toAdd to offsetForElement[elementNo], as offsetCnt[] if not present
void addToOffset(int **offsetForElement, int elementNo, int *offsetCnt, int toAdd) {
	int index = 0;
	int alreadyExist = 0;
	//search existing
	for(index=0; index<*offsetCnt; index ++) {
		if (offsetForElement[elementNo][index] == toAdd ) {
			alreadyExist = 1;
			break;
		}
	}
	//add if not present already
	if (alreadyExist == 0) {
		offsetForElement[elementNo][*offsetCnt] = toAdd;
		*offsetCnt = *offsetCnt + 1;
	}
}

// getPlainTextOrCipherText
//if getPlainText == true, get plain text
// otherwise,				get cipher text 
void getPlainTextOrCipherText(char **allStrips, int *keyGroup, char * ciphertext, char *plaintext, int offset, int cipherTextLength, int getPlainText) 
{
	static int id;
	int cipher_id = 0;
	int k = 0, keyNo = 0;
	char *keyStr = NULL;
	char cipher_char = 'A';
	int c = 0;
	//initialize to 000000
	for(cipher_id=0; cipher_id<cipherTextLength; cipher_id++) {
		plaintext[cipher_id] = '0';
	}

	//for each keyId, search it's plaintext (which is of distance -offset from ciphertext)
	for(cipher_id=0; cipher_id<cipherTextLength; cipher_id++){
		cipher_char = ciphertext[cipher_id];
		k = cipher_id % 25;
		keyNo = keyGroup[k];
		keyStr = allStrips[keyNo];
		if (getPlainText ) {
			for (c=offset; c<52; c++) {
				if (keyStr[c] == cipher_char) {
					plaintext[cipher_id] = keyStr[c-offset];
					break;
				}
			}
		}
		else {
			//get encrypt text; "cipher_char" is actually plain_text, "plaintext" is actually ciphertext
			for (c=0; c< 52 - offset; c++) {
				if (keyStr[c] == cipher_char) {
					plaintext[cipher_id] = keyStr[c+offset];
					break;
				}
			}
		}
	}

	//print for debugging 
	//if (getPlainText ) {
		//printf("Plain Text: %d: ", id ++);
	//}
	//else {
		//printf("Cipher Text: %d: ", id ++);
	//}
	//for(cipher_id=0; cipher_id<cipherTextLength; cipher_id++){
		//printf("%c", plaintext[cipher_id]);
	//}
	//printf("\n");
}

//whether already inside the key group (tmpKeyGroup) of size tmpKeyCnt
int alreadyExistInKeygroup(int *keyGroup, int keyGroupLength, int newId) {
	int i = 0;
	for(i=0; i<keyGroupLength; i++) {
		if(newId == keyGroup[i]) 
			return 1;	
	}
	return 0;
}

//getKeysGroup			get every group of keys
void getKeysGroup(int **potentialKeysId,  int *potentialKeysCnt, int *tmpKeyGroup, int tmpKeyId, 
		int **keyGroup, int *keyGroupCnt) 
{
	int i = 0;
	if ( tmpKeyId == 25) {	//a complete set of key was found, record it
		keyGroup[*keyGroupCnt] = (int*)calloc(25, sizeof(int));
		for(i=0; i<25; i++) {
			keyGroup[*keyGroupCnt][i] = tmpKeyGroup[i];
		}
		*keyGroupCnt = *keyGroupCnt + 1;
		return;
	}
	//insert one key to tmpKeyGroup, then move next
	for(i=0; i<potentialKeysCnt[tmpKeyId]; i++ ){
		if (alreadyExistInKeygroup(tmpKeyGroup, tmpKeyId, potentialKeysId[tmpKeyId][i]) ) {
			continue;
		}
		tmpKeyGroup[tmpKeyId] = potentialKeysId[tmpKeyId][i];
		getKeysGroup(potentialKeysId, potentialKeysCnt, tmpKeyGroup, tmpKeyId + 1, keyGroup, keyGroupCnt);
	}
}

//check wether newId is alreayd present somewhere inside array potentialKeysId[0:stripCnt]
int alreadyAddedAsKey(int **potentialKeysId, int *potentialKeysCnt, int stripCnt, int NewId) {
	
	int i = 0, j = 0;
	for(i=0; i<stripCnt; i++) {
		for(j=0; j<potentialKeysCnt[i]; j++) {
			if (potentialKeysId[i][j] == NewId )
				return 1;
		}
	}
	return 0;
}

//decrypt()							given ciphertext[], print possible plain texts
//allStrips							all strips that may potentially act as key strs
//potentialKeysId[keyNo = 0:24]  	stripId(0-99) which may act as the keyNo key
//potentialkeysCnt				 	number of potential keys at index keyNo
//offset							dist between plain and cipher text (plain text first)
//inputStr							original string to be decrypt (if isDecrypt=true) or encrypt(otherwise)
//isDecrypt							if true, decrypt inputStr; otherwise, encrypt
void decrypt(char **allStrips, char *ciphertext, int **potentialKeysId, int *potentialKeysCnt, int offset,
		char *inputStr, int isDecrypt) 
{
	//for first letter inside ciphertext
	//		get strip[stripId], where stripId = potentialKeys[0]
	int keyGroupCnt = 0;
	int i = 0, j = 0, k = 0;
	int *tmpKeyGroup = (int *)calloc(100, sizeof(int ));	//tmp storage of a group of key

	int **keyGroup = (int **)calloc(500, sizeof(int *));
	char plaintext[1024];
	char tmpContainer[1024];	 //temp storage
	char newCipherText[]="BBQGFHSDXNFKLXRREYYPADREWTFRJGJDCBGDZFXINXMWYLHTGPAXHOLTHXPRCTTADFWOJYXAEYRNKRXRXDKHSFDUVPXQGWMKMYKZ";
	int correctKeyId = 0;
	char newPlainText[] = "TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQUOTE";
	int strLength = 0;	 //length of final string

	for(i=0; i<500; i++) {
		keyGroup[i] = NULL;	//to be initialized once a whole group of key is found
	}
	getKeysGroup(potentialKeysId, potentialKeysCnt,  tmpKeyGroup, 0, keyGroup, &keyGroupCnt);
	
	//print for debugging
	//for(i=0; i<keyGroupCnt; i++) {
		//printf("key group %d: ", i);
		//for(j=0; j<25; j++) {
			//printf(" %d ", keyGroup[i][j]);
		//}
		//printf("\n");
	//}
	
	//now with each group of keys, find the plain text

	//find the plain text (char) according to keyId, and offset	
	for(k=0; k<keyGroupCnt; k++) {
		//getPlainText(allStrips, keyGroup[k], ciphertext, plaintext, offset, 48);
		getPlainTextOrCipherText(allStrips, keyGroup[k], newCipherText, plaintext, offset, 100, 1);
		plaintext[100] = '\0';
		if (strcmp(plaintext, "TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQUOTE") == 0) {
			//this key is the correct one
			correctKeyId = k;
		}
	}
	
	//encrypt for double check
	//find the cipher text (char) according to keyId, and offset	
	getPlainTextOrCipherText(allStrips, keyGroup[correctKeyId], newPlainText, plaintext, offset, 100, 0);

	//finally operate on input str
	strLength = strlen(inputStr);
	getPlainTextOrCipherText(allStrips, keyGroup[correctKeyId], inputStr, plaintext, offset, strLength, isDecrypt);
	plaintext[strLength] = '\0';
	
	//double check by reverse the results (plaintext[]) 
	getPlainTextOrCipherText(allStrips, keyGroup[correctKeyId], plaintext, tmpContainer, offset, strLength, !isDecrypt);
	tmpContainer[strLength] = '\0';
	//now tmpContainer should be equal to inputStr
	printf("Input string is \n\t%s \n", inputStr);
	if (isDecrypt) {
		printf("After decrption, new string is \n\t%s \n", plaintext);
		printf("For double check only: after re-encrption, string (should be equal to input string) is \n\t%s \n", tmpContainer);
	}
	else {
		printf("After encrption, new string is \n\t%s \n", plaintext);
		printf("For double check only: after re-decrption, string (should be equal to input string) is \n\t%s \n", tmpContainer);
	}
	

	//free memory
	for(i=0; i<500; i++) {
		if (keyGroup[i]) {
			free (keyGroup[i]);
		}
	}
	free (keyGroup);
	free (tmpKeyGroup);
}

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

	//read in inputs
	i = 0; 
	while (i < argc) {
		if (strcmp(argv[i], "-decrypt") == 0) {
			isDecrypt = 1;
		}

		if (strcmp(argv[i], "-encrypt") == 0) {
			isDecrypt = 0;
		}
		if (strcmp(argv[i], "-input") == 0) {
			if (i+1 >=argc ) {
				printf("Error! There must be input str after \"-input\".\n");
				exit(0);
			}
			if (strlen(argv[i+1]) > 1024) {	
				printf("Error! Input string following -input can not be of length > 1024.\n");
			}
			strcpy(inputStr, argv[i+1]);
			//if (strlen(inputStr) > 100) {
				//printf("Warning! Input string is of length > 100; will be shorted to 100 chars.\n");
				//inputStr[100] = '\0';
			//}
			//else if (strlen(inputStr) < 100) {
				//printf("Warning! Input string is of length < 100; will be appended by 'O' to 100 chars.\n");
				//for( i=strlen(inputStr); i<100; i++ ) {
					//inputStr[i] = 'O';
				//}
				//inputStr[100] = '\0';
			//}

			printf("Note: all lowercase characters (if any) will be moved to upcase. \n");
			for( i=0; i<strlen(inputStr); i++ ) {
				if (inputStr[i] >='a' && inputStr[i] <='z') {
					inputStr[i] = inputStr[i] + 'A'-'a';
				}
			}
		}
		i ++;
	}
	
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

	offsetForElement = (int **)calloc(N, sizeof(int *));
	for(i=0; i<N; i++) {
		offsetForElement[i] = (int *)calloc(100, sizeof(int));
	}

	potentialKeysId = (int **)calloc(N, sizeof(int *));
	for(i=0; i<N; i++) {
		potentialKeysId[i] = (int *)calloc(100, sizeof(int));
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
	
	// the for loop below is to iterate all the strips for looking for which
	// key works for the character
	// because we know the first 48 ciphertext, so we can use the keyStrips
	// from 0-22 are same as 25-47
	// and keyStrip of the place at 0 = at 25, at 1 = at26, ..., at 22 = at
	// 47. So, the distance between the character of
	// plaintext and the corresponding character of ciphertext shall equal
	for (stripsNo = 0; stripsNo < 100; stripsNo++) {
		// the for loop below is looking for the common offset to the first
		// 23 character in ciphertext
		for (elementNo = 0; elementNo < 23; elementNo++) {
			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == plaintext[elementNo]) {
					index_plaintext = i;
					break;
				}
			}

			for (i = 0; i < 52; i++) {
					if (strips[stripsNo][i] == ciphertext[elementNo]) {
					index_ciphertext = i;
					break;
				}
			}
			d1 = index_ciphertext - index_plaintext;
			d11 = d1 + 26;

			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == plaintext[elementNo + 25]) {
					index_p2 = i;
					break;
				}
			}

			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == ciphertext[elementNo + 25]) {
					index_c2 = i;
					break;
				}

			}

			d2 = index_c2 - index_p2;
			d22 = d2 + 26;
			if (d1 == d2 || d1 == d22 || d2 == d11) {
				if (d1 == d2 && d1 > 0)
					addToOffset(offsetForElement, elementNo, &offsetCnt[elementNo],  d1);

				if (d1 == d2 && d1 < 0)
					//offsetForElement[elementNo][offsetCnt[elementNo]++] = d1 + 26;
					addToOffset(offsetForElement, elementNo, &offsetCnt[elementNo],  d1+26);

				if (d1 == d22)
					//offsetForElement[elementNo][offsetCnt[elementNo]++] = d1;
					addToOffset(offsetForElement, elementNo, &offsetCnt[elementNo],  d1);

				if (d2 == d11)
					//offsetForElement[elementNo][offsetCnt[elementNo]++] = d2;
					addToOffset(offsetForElement, elementNo, &offsetCnt[elementNo],  d2);
			}
		} //Elements
	}// iterate all strips

	//find the common (after removing duplicates), which occured 23 times
	for (elementNo = 0; elementNo < 23; elementNo++) {
		//printf("element %d: ", elementNo);
		//for(i=0; i<offsetCnt[elementNo]; i++) {
			//printf(" %d ", offsetForElement[elementNo][i]);
		//}
		//printf("\n");

		for( i=0; i<offsetCnt[elementNo]; i++) {
			thisNum = offsetForElement[elementNo][i];
			findPrevious = 0;
			for(j=0; j<i; j++) {
				if (thisNum == offsetForElement[elementNo][j] ) {
					findPrevious = 1;
					break;
				}
			}
			if (findPrevious== 0) {
				offsetFoundTimes[thisNum] ++;
			}
		}
	}
		
	//find which is the offset
	for (i = 0; i < 100; i++) {
		if (offsetFoundTimes[i] == 23) {
			globalOffset = i;
			//printf(" offsetFondTimes[%d] = %d, \n", i, offsetFoundTimes[i]);
		}
	}

	//step 3: find all slips that match the first/second/... cipher-plain text 
	for (stripsNo = 0; stripsNo < 100; stripsNo++) {
		//here only search 23 keys; 24 and 25 will be added later
		for (elementNo = 0; elementNo < 23; elementNo++) {
			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == plaintext[elementNo]) {
					index_plaintext = i;
					break;
				}
			}

			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == ciphertext[elementNo]) {
					index_ciphertext = i;
					break;
				}
			}
			d1 = index_ciphertext - index_plaintext;
			d11 = d1 + 26;

			for (i = 0; i < 52; i++) {
				if (strips[stripsNo][i] == plaintext[elementNo + 25]) {
					index_p2 = i;
					break;
				}
			}

			for ( i = 0; i < 52;i++) {
				if (strips[stripsNo][i] == ciphertext[elementNo + 25]) {
					index_c2 = i;
					break;
				}
			}

			d2 = index_c2 - index_p2;
			d22 = d2 + 26;
			if (d1 == d2 || d1 == d22 || d2 == d11) {
				if (d1 == d2 && d1 > 0 && d1 == 12)
					addToOffset(potentialKeysId, elementNo, &potentialKeysCnt[elementNo],  stripsNo);

				if (d1 == d2 && d1 < 0 && d1 + 26 == 12)
					addToOffset(potentialKeysId, elementNo, &potentialKeysCnt[elementNo],  stripsNo);

				if (d1 == d22 && d1 == 12)
					addToOffset(potentialKeysId, elementNo, &potentialKeysCnt[elementNo],  stripsNo);

				if (d2 == d11 && d2 == 12)
				addToOffset(potentialKeysId, elementNo, &potentialKeysCnt[elementNo],  stripsNo);
			}
		} //Elements
	}// iterate all strips

	//continue to search for key 24 and key 25 
	//Note that for the correct strip, dist of cipher-plain is globalOffset
	for(elementNo=23; elementNo<25; elementNo ++) {
		for (stripsNo = 0; stripsNo < 100; stripsNo++) {
		//don't visit the strip if already added (as key 0-22) 
			//	don't exclude the key strips inside [23]
			if (alreadyAddedAsKey(potentialKeysId, potentialKeysCnt, (elementNo>23? 23:elementNo), stripsNo) ) {
				continue;
			}
			
			for(c=globalOffset; c<52; c++) {
				if (strips[stripsNo][c] 			 == ciphertext[elementNo] && 
					strips[stripsNo][c-globalOffset] ==  plaintext[elementNo]) {
					//printf("strips %d: c is %d, c-offset %d, [c][off] %c %c, ciphier, plain %c %c\n",
						//stripsNo, c, c-globalOffset, strips[stripsNo][c], strips[stripsNo][c-globalOffset],
						//ciphertext[elementNo], plaintext[elementNo]);
					addToOffset(potentialKeysId, elementNo, &potentialKeysCnt[elementNo],  stripsNo);
				}
			}
		}	
	}
	//debugging 
	//for (elementNo = 0; elementNo < 25; elementNo++) {
		//printf("potential keysId for element %d: ", elementNo);
		//for(i=0; i<potentialKeysCnt[elementNo]; i++) {
				//printf(" %d ", potentialKeysId[elementNo][i]);
		//}
		//printf("\n");
	//}


	//now try each of the potential keys, and then decrypt cipher to plaintext
	//		then manually selection / decision is needed to finalize the potentialKeysId
	//printf("now going to decrypt.\n");
	decrypt(strips, ciphertext, potentialKeysId, potentialKeysCnt, globalOffset, inputStr, isDecrypt);

	for(i=0; i<N; i++) {
		free(strips[i]);
	}
	free(strips);

	free(offsetFoundTimes);
	for(i=0; i<N; i++) {
		free(potentialKeysId[i]);
	}
	free(potentialKeysId);
	free(potentialKeysCnt);

	free(offsetCnt);
	for(i=0; i<N; i++) {
		free(offsetForElement[i]);
	}
	free(offsetForElement);
	
	return 0;
}

