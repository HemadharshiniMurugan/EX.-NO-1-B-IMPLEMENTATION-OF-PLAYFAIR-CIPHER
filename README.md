# EX.-NO-1-B-IMPLEMENTATION-OF-PLAYFAIR-CIPHER

## AIM:
  To write a C program to implement the Playfair Substitution technique.
  
## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyTable(char key[], char keyTable[SIZE][SIZE]) {
    int dict[26] = {0};
    int i, j, k;
    int len = strlen(key);
    
    k = 0;
    for (i = 0; i < len; i++) {
        if (key[i] != 'j' && dict[key[i] - 'a'] == 0) {
            keyTable[k / SIZE][k % SIZE] = key[i];
            dict[key[i] - 'a'] = 1;
            k++;
        }
    }

    for (i = 0; i < 26; i++) {
        if (i != 9 && dict[i] == 0) { // i != 9 excludes 'j'
            keyTable[k / SIZE][k % SIZE] = i + 'a';
            k++;
        }
    }
}

void searchInTable(char keyTable[SIZE][SIZE], char a, char b, int pos[]) {
    int i, j;

    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                pos[0] = i;
                pos[1] = j;
            }
            if (keyTable[i][j] == b) {
                pos[2] = i;
                pos[3] = j;
            }
        }
    }
}

void encrypt(char plaintext[], char keyTable[SIZE][SIZE]) {
    int pos[4];
    int i;
    int len = strlen(plaintext);

    for (i = 0; i < len; i += 2) {
        if (plaintext[i] == plaintext[i + 1]) {
            plaintext[i + 1] = 'x';
        }

        searchInTable(keyTable, plaintext[i], plaintext[i + 1], pos);

        if (pos[0] == pos[2]) {
            plaintext[i] = keyTable[pos[0]][(pos[1] + 1) % SIZE];
            plaintext[i + 1] = keyTable[pos[2]][(pos[3] + 1) % SIZE];
        } else if (pos[1] == pos[3]) {
            plaintext[i] = keyTable[(pos[0] + 1) % SIZE][pos[1]];
            plaintext[i + 1] = keyTable[(pos[2] + 1) % SIZE][pos[3]];
        } else {
            plaintext[i] = keyTable[pos[0]][pos[3]];
            plaintext[i + 1] = keyTable[pos[2]][pos[1]];
        }
    }
}

void prepareText(char text[], char preparedText[]) {
    int i, j;
    int len = strlen(text);

    for (i = 0, j = 0; i < len; i++) {
        if (text[i] == 'j') text[i] = 'i';

        if (isalpha(text[i])) {
            preparedText[j++] = tolower(text[i]);
            if (j % 2 == 0 && preparedText[j - 1] == preparedText[j - 2]) {
                preparedText[j++] = 'x';
            }
        }
    }
    if (j % 2 != 0) {
        preparedText[j++] = 'x';
    }
    preparedText[j] = '\0';
}

int main() {
    char key[SIZE * SIZE], keyTable[SIZE][SIZE];
    char text[100], preparedText[100];

    printf("Key text: ");
    scanf("%s", key);

    generateKeyTable(key, keyTable);

    printf("Plain text: ");
    scanf("%s", text);

    prepareText(text, preparedText);

    encrypt(preparedText, keyTable);

    printf("Ciphertext: %s\n", preparedText);

    return 0;
}
```
## OUTPUT:

![crypto ex2 out](https://github.com/user-attachments/assets/cc219fad-10e9-4bfb-a5a5-4145860e185b)


## RESULT:
  Thus the Playfair cipher substitution technique had been implemented successfully.
