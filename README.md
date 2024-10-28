# EX.-NO-1-C-IMPLEMENTATION-OF-HILL-CIPHER

## AIM:
To write a C program to implement the hill cipher substitution techniques.

## ALGORITHM:

STEP-1: Read the plain text and key from the user.

STEP-2: Split the plain text into groups of length three.

STEP-3: Arrange the keyword in a 3*3 matrix.

STEP-4: Multiply the two matrices to obtain the cipher text of length three.

STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM: 
```
#include <stdio.h>
#include <string.h>

#define SIZE 2  // Block size for 2x2 matrix

// Function to calculate modular inverse
int modInverse(int a, int m) {
    a = a % m;
    for (int x = 1; x < m; x++) {
        if ((a * x) % m == 1)
            return x;
    }
    return -1;
}

// Function to find the determinant of a 2x2 matrix
int determinant(int matrix[SIZE][SIZE]) {
    return matrix[0][0] * matrix[1][1] - matrix[0][1] * matrix[1][0];
}

// Function to find the adjoint of a 2x2 matrix
void adjoint(int matrix[SIZE][SIZE], int adj[SIZE][SIZE]) {
    adj[0][0] = matrix[1][1];
    adj[1][1] = matrix[0][0];
    adj[0][1] = -matrix[0][1];
    adj[1][0] = -matrix[1][0];
}

// Function to find the inverse of a 2x2 matrix mod 26
int inverseMatrix(int matrix[SIZE][SIZE], int inverse[SIZE][SIZE]) {
    int det = determinant(matrix);
    int det_inv = modInverse(det % 26, 26);
    if (det_inv == -1) {
        printf("Inverse doesn't exist\n");
        return 0;
    }

    int adj[SIZE][SIZE];
    adjoint(matrix, adj);

    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            inverse[i][j] = (adj[i][j] * det_inv) % 26;
            if (inverse[i][j] < 0)
                inverse[i][j] += 26;
        }
    }
    return 1;
}

// Function to perform matrix multiplication and mod 26 for encryption and decryption
void matrixMultiply(int key[SIZE][SIZE], int text[SIZE], int result[SIZE]) {
    for (int i = 0; i < SIZE; i++) {
        result[i] = 0;
        for (int j = 0; j < SIZE; j++) {
            result[i] += key[i][j] * text[j];
        }
        result[i] %= 26;
    }
}

// Function to encrypt plaintext using Hill cipher
void encrypt(int key[SIZE][SIZE], char *plaintext, char *ciphertext) {
    int textVector[SIZE];
    int cipherVector[SIZE];
    int len = strlen(plaintext);
    if (len % SIZE != 0) strcat(plaintext, "X");  // Padding if necessary

    for (int i = 0; i < len; i += SIZE) {
        for (int j = 0; j < SIZE; j++) {
            textVector[j] = plaintext[i + j] - 'A';
        }
        matrixMultiply(key, textVector, cipherVector);
        for (int j = 0; j < SIZE; j++) {
            ciphertext[i + j] = cipherVector[j] + 'A';
        }
    }
    ciphertext[len] = '\0';
}

// Function to decrypt ciphertext using Hill cipher
void decrypt(int key[SIZE][SIZE], char *ciphertext, char *decryptedtext) {
    int inverseKey[SIZE][SIZE];
    if (!inverseMatrix(key, inverseKey)) return;

    int textVector[SIZE];
    int decipheredVector[SIZE];
    int len = strlen(ciphertext);

    for (int i = 0; i < len; i += SIZE) {
        for (int j = 0; j < SIZE; j++) {
            textVector[j] = ciphertext[i + j] - 'A';
        }
        matrixMultiply(inverseKey, textVector, decipheredVector);
        for (int j = 0; j < SIZE; j++) {
            decryptedtext[i + j] = decipheredVector[j] + 'A';
        }
    }
    decryptedtext[len] = '\0';
}

int main() {
    char plaintext[128] = "LOKESH";
    char ciphertext[128];
    char decryptedtext[128];

    // 2x2 Key Matrix
    int key[SIZE][SIZE] = {
        {3, 3},
        {2, 5}
    };

    // Ensure plaintext is uppercase
    for (int i = 0; plaintext[i]; i++) {
        plaintext[i] = toupper(plaintext[i]);
    }

    printf("Plaintext: %s\n", plaintext);

    // Encrypt
    encrypt(key, plaintext, ciphertext);
    printf("Encrypted text: %s\n", ciphertext);

    // Decrypt
    decrypt(key, ciphertext, decryptedtext);
    printf("Decrypted text: %s\n", decryptedtext);

    return 0;
}

```

## OUTPUT:

![image](https://github.com/user-attachments/assets/3b390f21-7034-42d6-87fd-aa1443cba881)




## RESULT:
  Thus the hill cipher substitution technique had been implemented successfully.
