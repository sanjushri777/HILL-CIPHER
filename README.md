# HILL CIPHER
HILL CIPHER
EX. NO: 3 AIM:
 

IMPLEMENTATION OF HILL CIPHER
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user.
STEP-2: Split the plain text into groups of length three. 
STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 
```C
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 1000

// Key matrix (3x3) for encryption
int keymat[3][3] = {
    {1, 2, 1},
    {2, 3, 2},
    {2, 2, 1}
};

// Inverse key matrix mod 26 for decryption
int invkeymat[3][3] = {
    {-1, 0, 1},
    {2, -1, 0},
    {-2, 2, -1}
};

char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Convert a char to uppercase and 0-25 index
int charToInt(char c) {
    return toupper(c) - 'A';
}

// Convert 0-25 index to uppercase char
char intToChar(int i) {
    return key[i % 26];
}

// Multiply vector by matrix mod 26
void multiplyMatrixVector(int mat[3][3], int vec[3], int res[3]) {
    for (int i = 0; i < 3; i++) {
        res[i] = 0;
        for (int j = 0; j < 3; j++) {
            res[i] += mat[i][j] * vec[j];
        }
        res[i] = ((res[i] % 26) + 26) % 26;  // mod 26 and handle negatives
    }
}

// Encrypt 3 letters block
void encryptBlock(char input[3], char output[3]) {
    int vec[3], res[3];
    for (int i = 0; i < 3; i++) {
        vec[i] = charToInt(input[i]);
    }
    multiplyMatrixVector(keymat, vec, res);
    for (int i = 0; i < 3; i++) {
        output[i] = intToChar(res[i]);
    }
}

// Decrypt 3 letters block
void decryptBlock(char input[3], char output[3]) {
    int vec[3], res[3];
    for (int i = 0; i < 3; i++) {
        vec[i] = charToInt(input[i]);
    }
    multiplyMatrixVector(invkeymat, vec, res);
    for (int i = 0; i < 3; i++) {
        output[i] = intToChar(res[i]);
    }
}

int main() {
    char msg[SIZE], paddedMsg[SIZE], enc[SIZE] = "", dec[SIZE] = "";
    int len;

    printf("enter the plaintext (A-Z only): ");
    fgets(msg, SIZE, stdin);

    // Remove newline and convert to uppercase
    len = strlen(msg);
    if (msg[len - 1] == '\n') msg[len - 1] = '\0';

    for (int i = 0; msg[i]; i++) {
        if (msg[i] == ' ') {
            for (int j = i; j < len; j++) msg[j] = msg[j+1];  // Remove spaces
            len--;
            i--;
        } else {
            msg[i] = toupper(msg[i]);
        }
    }

    len = strlen(msg);

    // Padding with 'X' to make length divisible by 3
    strcpy(paddedMsg, msg);
    int padCount = 0;
    while (strlen(paddedMsg) % 3 != 0) {
        paddedMsg[len + padCount] = 'X';
        padCount++;
        paddedMsg[len + padCount] = '\0';
    }
    len = strlen(paddedMsg);

    printf("Padded plaintext: %s\n", paddedMsg);

    // Encrypt message in blocks of 3
    for (int i = 0; i < len; i += 3) {
        char block[3];
        char encBlock[3];
        for (int j = 0; j < 3; j++) block[j] = paddedMsg[i + j];
        encryptBlock(block, encBlock);
        encBlock[3] = '\0';
        strcat(enc, encBlock);
    }

    printf("Encrypted text: %s\n", enc);

    // Decrypt message in blocks of 3
    for (int i = 0; i < strlen(enc); i += 3) {
        char block[3];
        char decBlock[3];
        for (int j = 0; j < 3; j++) block[j] = enc[i + j];
        decryptBlock(block, decBlock);
        decBlock[3] = '\0';
        strcat(dec, decBlock);
    }

    printf("Decrypted text: %s\n", dec);

    return 0;
}

```

## OUTPUT

![image](https://github.com/user-attachments/assets/acdcfa98-2ecb-429a-b134-8e0c9bf54184)


## RESULT

The IMPLEMENTATION OF HILL CIPHER  program is executed successfully.
