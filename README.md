# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
## CaearCipher.
~~~
#include <stdio.h>
#include <ctype.h>

void caesar_cipher(char *text, int shift, int encrypt) {
    int i;

    for (i = 0; text[i] != '\0'; i++) {
        if (isalpha(text[i])) {
            char base = isupper(text[i]) ? 'A' : 'a';
            text[i] = (char)((text[i] - base + (encrypt ? shift : -shift)) % 26 + base);
        }
    }
}

int main() {
    char text[100];
    int shift;

    printf("Enter the text: ");
    fgets(text, 100, stdin);

    printf("Enter the shift value: ");
    scanf("%d", &shift);

    caesar_cipher(text, shift, 1); // Encrypt

    printf("Encrypted text: %s\n", text);

    caesar_cipher(text, shift, 0); // Decrypt

    printf("Decrypted text: %s\n", text);

    return 0;
}

~~~
## OUTPUT:
![Screenshot 2024-11-17 111516](https://github.com/user-attachments/assets/97324243-37d0-44b7-9d19-e95898b33dbb)


## RESULT:
The program is executed successfully

---------------------------------

## PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
## PlayFair Cipher
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void prepareKeyMatrix(char key[], char keyMatrix[SIZE][SIZE]) {
   int used[26] = {0};
   int i, j, k = 0;

   used['J' - 'A'] = 1;

   for (i = 0; key[i] != '\0'; i++) {
       char currentChar = toupper(key[i]);
       if (!used[currentChar - 'A']) {
           keyMatrix[k / SIZE][k % SIZE] = currentChar;
           used[currentChar - 'A'] = 1;
           k++;
       }
   }

   for (i = 0; i < 26; i++) {
       if (!used[i]) {
           keyMatrix[k / SIZE][k % SIZE] = 'A' + i;
           k++;
       }
   }
}

void printKeyMatrix(char keyMatrix[SIZE][SIZE]) {
   printf("Key Matrix:\n");
   for (int i = 0; i < SIZE; i++) {
       for (int j = 0; j < SIZE; j++) {
           printf("%c ", keyMatrix[i][j]);
       }
       printf("\n");
   }
}

void findPosition(char keyMatrix[SIZE][SIZE], char letter, int *row, int *col) {
   if (letter == 'J') letter = 'I';
   for (int i = 0; i < SIZE; i++) {
       for (int j = 0; j < SIZE; j++) {
           if (keyMatrix[i][j] == letter) {
               *row = i;
               *col = j;
               return;
           }
       }
   }
}

void encrypt(char text[], char keyMatrix[SIZE][SIZE], char encryptedText[]) {
   int i, row1, col1, row2, col2;
   char first, second;
   int length = strlen(text);

   for (i = 0; i < length; i += 2) {
       first = toupper(text[i]);
       second = (i + 1 < length) ? toupper(text[i + 1]) : 'X';

       findPosition(keyMatrix, first, &row1, &col1);
       findPosition(keyMatrix, second, &row2, &col2);

       if (row1 == row2) {
           encryptedText[i] = keyMatrix[row1][(col1 + 1) % SIZE];
           encryptedText[i + 1] = keyMatrix[row2][(col2 + 1) % SIZE];
       } else if (col1 == col2) {
           encryptedText[i] = keyMatrix[(row1 + 1) % SIZE][col1];
           encryptedText[i + 1] = keyMatrix[(row2 + 1) % SIZE][col2];
       } else {
           encryptedText[i] = keyMatrix[row1][col2];
           encryptedText[i + 1] = keyMatrix[row2][col1];
       }
   }
   encryptedText[length] = '\0';
}

void decrypt(char text[], char keyMatrix[SIZE][SIZE], char decryptedText[]) {
   int i, row1, col1, row2, col2;
   char first, second;
   int length = strlen(text);

   for (i = 0; i < length; i += 2) {
       first = toupper(text[i]);
       second = (i + 1 < length) ? toupper(text[i + 1]) : 'X';

       findPosition(keyMatrix, first, &row1, &col1);
       findPosition(keyMatrix, second, &row2, &col2);

       if (row1 == row2) {
           decryptedText[i] = keyMatrix[row1][(col1 - 1 + SIZE) % SIZE];
           decryptedText[i + 1] = keyMatrix[row2][(col2 - 1 + SIZE) % SIZE];
       } else if (col1 == col2) {
           decryptedText[i] = keyMatrix[(row1 - 1 + SIZE) % SIZE][col1];
           decryptedText[i + 1] = keyMatrix[(row2 - 1 + SIZE) % SIZE][col2];
       } else {
           decryptedText[i] = keyMatrix[row1][col2];
           decryptedText[i + 1] = keyMatrix[row2][col1];
       }
   }
   decryptedText[length] = '\0';
}

int main() {
   char key[100], text[100], keyMatrix[SIZE][SIZE], encryptedText[100], decryptedText[100];

   printf("Enter the keyword: ");
   fgets(key, sizeof(key), stdin);
   key[strcspn(key, "\n")] = '\0';

   printf("Enter the plaintext: ");
   fgets(text, sizeof(text), stdin);
   text[strcspn(text, "\n")] = '\0';

   if (strlen(text) % 2 != 0) {
       strcat(text, "X");
   }

   prepareKeyMatrix(key, keyMatrix);
   printKeyMatrix(keyMatrix);

   printf("Plaintext: %s\n", text);

   encrypt(text, keyMatrix, encryptedText);
   printf("Encrypted Text: %s\n", encryptedText);

   decrypt(encryptedText, keyMatrix, decryptedText);
   printf("Decrypted Text: %s\n", decryptedText);

   return 0;
}

~~~

## OUTPUT:
![Screenshot 2024-11-17 112432](https://github.com/user-attachments/assets/342f1628-e222-4065-b16b-3f49822b3e17)




## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
## Hill Cipher
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int keymat[3][3] = {{1, 2, 1}, {2, 3, 2}, {2, 2, 1}};
int invkeymat[3][3] = {{-1, 0, 1}, {2, -1, 0}, {-2, 2, -1}};
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

void encode(char a, char b, char c, char *ret) {
    int x = (a - 65) * keymat[0][0] + (b - 65) * keymat[1][0] + (c - 65) * keymat[2][0];
    int y = (a - 65) * keymat[0][1] + (b - 65) * keymat[1][1] + (c - 65) * keymat[2][1];
    int z = (a - 65) * keymat[0][2] + (b - 65) * keymat[1][2] + (c - 65) * keymat[2][2];
    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

void decode(char a, char b, char c, char *ret) {
    int x = (a - 65) * invkeymat[0][0] + (b - 65) * invkeymat[1][0] + (c - 65) * invkeymat[2][0];
    int y = (a - 65) * invkeymat[0][1] + (b - 65) * invkeymat[1][1] + (c - 65) * invkeymat[2][1];
    int z = (a - 65) * invkeymat[0][2] + (b - 65) * invkeymat[1][2] + (c - 65) * invkeymat[2][2];
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
}

int main() {
    char msg[1000], enc[1000] = "", dec[1000] = "", temp[4];

    printf("Enter the plaintext: ");
    fgets(msg, sizeof(msg), stdin);
    msg[strcspn(msg, "\n")] = '\0';

    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    int original_len = strlen(msg);

    int n = original_len % 3;
    if (n != 0) {
        for (int i = 0; i < 3 - n; i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message: %s\n", msg);

    for (int i = 0; i < strlen(msg); i += 3) {
        encode(msg[i], msg[i + 1], msg[i + 2], temp);
        strcat(enc, temp);
    }

    for (int i = 0; i < strlen(enc); i += 3) {
        decode(enc[i], enc[i + 1], enc[i + 2], temp);
        strcat(dec, temp);
    }

    dec[original_len] = '\0';

    printf("Encoded message: %s\n", enc);
    printf("Decoded message: %s\n", dec);

    return 0;
}

~~~
## OUTPUT:
![Screenshot 2024-11-17 112740](https://github.com/user-attachments/assets/d3966e5f-20b3-432f-8ebe-58266c071924)





Input Message : SecurityLaboratory
Padded Message : SECURITYLABORATORY Encrypted Message : EACSDKLCAEFQDUKSXU Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
## Vigenere Cipher
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

void vigenere_encrypt(const char *plaintext, const char *key, char *encrypted) {
    int pt_len = strlen(plaintext);
    int key_len = strlen(key);
    int i, j;

    for (i = 0, j = 0; i < pt_len; i++) {
        if (isalpha(plaintext[i])) {
            char pt_char = toupper(plaintext[i]);
            char key_char = toupper(key[j % key_len]);
            encrypted[i] = ((pt_char - 'A') + (key_char - 'A')) % 26 + 'A';
            j++;
        } else {
            encrypted[i] = plaintext[i];
        }
    }
    encrypted[pt_len] = '\0';
}

void vigenere_decrypt(const char *encrypted, const char *key, char *decrypted) {
    int enc_len = strlen(encrypted);
    int key_len = strlen(key);
    int i, j;

    for (i = 0, j = 0; i < enc_len; i++) {
        if (isalpha(encrypted[i])) {
            char enc_char = toupper(encrypted[i]);
            char key_char = toupper(key[j % key_len]);
            decrypted[i] = ((enc_char - 'A') - (key_char - 'A') + 26) % 26 + 'A';
            j++;
        } else {
            decrypted[i] = encrypted[i];
        }
    }
    decrypted[enc_len] = '\0';
}

int main() {
    char plaintext[1000], key[1000], encrypted[1000], decrypted[1000];

    printf("Enter the plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    printf("Enter the key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    vigenere_encrypt(plaintext, key, encrypted);
    printf("Encrypted message: %s\n", encrypted);

    vigenere_decrypt(encrypted, key, decrypted);
    printf("Decoded message: %s\n", decrypted);

    return 0;
}

~~~
## OUTPUT:
![Screenshot 2024-11-17 113029](https://github.com/user-attachments/assets/89e4d165-9083-4a0b-8660-9fae37fd3892)

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:
## Rail Fence Cipher
~~~
#include <stdio.h>
#include <string.h>

void encryptRailFence(char *plainText, int key, char *cipherText) {
    int len = strlen(plainText);
    int rail[key][len];
    memset(rail, 0, sizeof(rail));
    int dir_down = 0, row = 0, col = 0;

    for (int i = 0; i < len; i++) {
        if (row == 0 || row == key - 1) dir_down = !dir_down;
        rail[row][col++] = plainText[i];
        dir_down ? row++ : row--;
    }

    int index = 0;
    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            if (rail[i][j] != 0)
                cipherText[index++] = rail[i][j];

    cipherText[index] = '\0';
}

void decryptRailFence(char *cipherText, int key, char *plainText) {
    int len = strlen(cipherText);
    int rail[key][len];
    memset(rail, 0, sizeof(rail));
    int dir_down, row = 0, col = 0;

    for (int i = 0; i < len; i++) {
        if (row == 0) dir_down = 1;
        if (row == key - 1) dir_down = 0;
        rail[row][col++] = 1;
        dir_down ? row++ : row--;
    }

    int index = 0;
    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            if (rail[i][j] == 1 && index < len)
                rail[i][j] = cipherText[index++];

    dir_down = 0, row = 0, col = 0;
    for (int i = 0; i < len; i++) {
        if (row == 0) dir_down = 1;
        if (row == key - 1) dir_down = 0;
        if (rail[row][col] != 0) plainText[i] = rail[row][col++];
        dir_down ? row++ : row--;
    }

    plainText[len] = '\0';
}

int main() {
    char plainText[100], cipherText[100], decryptedText[100];
    int key;

    printf("Enter the plain text: ");
    fgets(plainText, sizeof(plainText), stdin);
    plainText[strcspn(plainText, "\n")] = '\0';

    printf("Enter the key (number of rails): ");
    scanf("%d", &key);

    encryptRailFence(plainText, key, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    decryptRailFence(cipherText, key, decryptedText);
    printf("Decrypted Text: %s\n", decryptedText);

    return 0;
}

~~~
## OUTPUT:
![Screenshot 2024-11-17 113130](https://github.com/user-attachments/assets/1d8fda7b-054d-4177-aef6-f924d11fba65)



## RESULT:
The program is executed successfully
