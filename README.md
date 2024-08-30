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
# ENCRYPTION:
```
#include <stdio.h>
#include <string.h>

void encryptCaesarCipher(char* message, int shift) {
    for (int i = 0; i < strlen(message); i++) {
        char c = message[i];
        
        // Check for uppercase letters
        if (c >= 'A' && c <= 'Z') {
            message[i] = ((c - 'A' + shift) % 26) + 'A';
        }
        // Check for lowercase letters
        else if (c >= 'a' && c <= 'z') {
            message[i] = ((c - 'a' + shift) % 26) + 'a';
        }
    }
}

int main() {
    char message[100];
    int shift;

    printf("Enter a message: ");
    fgets(message, sizeof(message), stdin);
    message[strcspn(message, "\n")] = 0;  // Remove newline character

    printf("Enter shift value: ");
    scanf("%d", &shift);

    // Encrypt the message
    encryptCaesarCipher(message, shift);
    printf("Encrypted Message: %s\n", message);

    return 0;
}
```

# DECRYPTION:
```
#include <stdio.h>
#include <string.h>

void decryptCaesarCipher(char* message, int shift) {
    for (int i = 0; i < strlen(message); i++) {
        char c = message[i];
        
        // Check for uppercase letters
        if (c >= 'A' && c <= 'Z') {
            message[i] = ((c - 'A' - shift + 26) % 26) + 'A';
        }
        // Check for lowercase letters
        else if (c >= 'a' && c <= 'z') {
            message[i] = ((c - 'a' - shift + 26) % 26) + 'a';
        }
    }
}

int main() {
    char message[100];
    int shift;

    printf("Enter an encrypted message: ");
    fgets(message, sizeof(message), stdin);
    message[strcspn(message, "\n")] = 0;  // Remove newline character

    printf("Enter shift value used for encryption: ");
    scanf("%d", &shift);

    // Decrypt the message
    decryptCaesarCipher(message, shift);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
```
## OUTPUT:
# ENCRYPTION
![image](https://github.com/user-attachments/assets/aafd619b-2128-4d9d-bb50-f54540edad7b)
# DECRYPTION
![image](https://github.com/user-attachments/assets/4ef9dc14-a7f4-454c-9307-851b8d3b1915)


## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
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
# ENCRYPTION:
```
#include <stdio.h>
#include <string.h>

#define SIZE 5

void createKeyMatrix(char key[], char keyMatrix[SIZE][SIZE]) {
    int dict[26] = {0};
    int k = 0, len = strlen(key);

    for (int i = 0; i < len; i++) {
        if (key[i] == 'J') key[i] = 'I'; // Replace 'J' with 'I'
        if (dict[key[i] - 'A'] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = key[i];
            dict[key[i] - 'A'] = 1;
            k++;
        }
    }

    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue; // Skip 'J'
        if (dict[ch - 'A'] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = ch;
            k++;
        }
    }
}

void processText(char* text) {
    for (int i = 0; i < strlen(text); i++) {
        if (text[i] == 'J') text[i] = 'I'; // Replace 'J' with 'I'
    }
}

void findPosition(char ch, char keyMatrix[SIZE][SIZE], int* row, int* col) {
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encryptPlayfairCipher(char* plaintext, char keyMatrix[SIZE][SIZE], char* ciphertext) {
    int row1, col1, row2, col2;
    int len = strlen(plaintext);

    for (int i = 0; i < len; i += 2) {
        findPosition(plaintext[i], keyMatrix, &row1, &col1);
        findPosition(plaintext[i+1], keyMatrix, &row2, &col2);

        if (row1 == row2) {
            ciphertext[i] = keyMatrix[row1][(col1 + 1) % SIZE];
            ciphertext[i+1] = keyMatrix[row2][(col2 + 1) % SIZE];
        } else if (col1 == col2) {
            ciphertext[i] = keyMatrix[(row1 + 1) % SIZE][col1];
            ciphertext[i+1] = keyMatrix[(row2 + 1) % SIZE][col2];
        } else {
            ciphertext[i] = keyMatrix[row1][col2];
            ciphertext[i+1] = keyMatrix[row2][col1];
        }
    }
    ciphertext[len] = '\0';
}

int main() {
    char key[] = "KEYWORD";
    char plaintext[] = "HELLO";
    char keyMatrix[SIZE][SIZE];
    char ciphertext[100];

    createKeyMatrix(key, keyMatrix);
    processText(plaintext);

    // Add padding if necessary
    if (strlen(plaintext) % 2 != 0) {
        strcat(plaintext, "X");
    }

    encryptPlayfairCipher(plaintext, keyMatrix, ciphertext);
    printf("Encrypted Message: %s\n", ciphertext);

    return 0;
}
```

# DECRYPTION
```
#include <stdio.h>
#include <string.h>

#define SIZE 5

void createKeyMatrix(char key[], char keyMatrix[SIZE][SIZE]) {
    int dict[26] = {0};
    int k = 0, len = strlen(key);

    for (int i = 0; i < len; i++) {
        if (key[i] == 'J') key[i] = 'I'; // Replace 'J' with 'I'
        if (dict[key[i] - 'A'] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = key[i];
            dict[key[i] - 'A'] = 1;
            k++;
        }
    }

    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue; // Skip 'J'
        if (dict[ch - 'A'] == 0) {
            keyMatrix[k / SIZE][k % SIZE] = ch;
            k++;
        }
    }
}

void findPosition(char ch, char keyMatrix[SIZE][SIZE], int* row, int* col) {
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void decryptPlayfairCipher(char* ciphertext, char keyMatrix[SIZE][SIZE], char* plaintext) {
    int row1, col1, row2, col2;
    int len = strlen(ciphertext);

    for (int i = 0; i < len; i += 2) {
        findPosition(ciphertext[i], keyMatrix, &row1, &col1);
        findPosition(ciphertext[i+1], keyMatrix, &row2, &col2);

        if (row1 == row2) {
            plaintext[i] = keyMatrix[row1][(col1 - 1 + SIZE) % SIZE];
            plaintext[i+1] = keyMatrix[row2][(col2 - 1 + SIZE) % SIZE];
        } else if (col1 == col2) {
            plaintext[i] = keyMatrix[(row1 - 1 + SIZE) % SIZE][col1];
            plaintext[i+1] = keyMatrix[(row2 - 1 + SIZE) % SIZE][col2];
        } else {
            plaintext[i] = keyMatrix[row1][col2];
            plaintext[i+1] = keyMatrix[row2][col1];
        }
    }
    plaintext[len] = '\0';
}

int main() {
    char key[] = "KEYWORD";
    char ciphertext[] = "KDMNK";
    char keyMatrix[SIZE][SIZE];
    char plaintext[100];

    createKeyMatrix(key, keyMatrix);

    decryptPlayfairCipher(ciphertext, keyMatrix, plaintext);
    printf("Decrypted Message: %s\n", plaintext);

    return 0;
}
```
## OUTPUT:
# ENCRYPTION
![image](https://github.com/user-attachments/assets/cb5a71ba-1834-43ec-9f9e-a84bbe04f9ed)
# DECRYPTION
![image](https://github.com/user-attachments/assets/d05a6afc-36bd-4baa-920a-ca95b56dacf0)


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
# ENCRYPTION
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

void encode(char a, char b, char c, char ret[4]) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

void encryptMessage(char msg[], char enc[]) {
    char temp[4];
    int n;

    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Padding the message if needed
    n = strlen(msg) % 3;
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    for (int i = 0; i < strlen(msg); i += 3) {
        encode(msg[i], msg[i + 1], msg[i + 2], temp);
        strcat(enc, temp);
    }
}

int main() {
    char msg[1000] = "SecurityLaboratory";
    char enc[1000] = "";

    printf("Input message : %s\n", msg);

    encryptMessage(msg, enc);

    printf("Encoded message : %s\n", enc);

    return 0;
}

```
# DECRYPTION
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

void decode(char a, char b, char c, char ret[4]) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 < 0) ? (26 + x % 26) : (x % 26)];
    ret[1] = key[(y % 26 < 0) ? (26 + y % 26) : (y % 26)];
    ret[2] = key[(z % 26 < 0) ? (26 + z % 26) : (z % 26)];
    ret[3] = '\0';
}

void decryptMessage(char enc[], char dec[]) {
    char temp[4];

    for (int i = 0; i < strlen(enc); i += 3) {
        decode(enc[i], enc[i + 1], enc[i + 2], temp);
        strcat(dec, temp);
    }
}

int main() {
    char enc[1000] = "UUGGXBKYAEGK";
    char dec[1000] = "";

    printf("Encoded message : %s\n", enc);

    decryptMessage(enc, dec);

    printf("Decoded message : %s\n", dec);

    return 0;
}

```



## OUTPUT:
# ENCRYPTION
![image](https://github.com/user-attachments/assets/72fb2c58-ac8b-490f-bbfa-a7012142fed8)

# DECRYPTION
![image](https://github.com/user-attachments/assets/2987fb5d-785c-4364-9d24-2b2a85954c86)

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
# ENCRYPTION
```
#include <stdio.h>
#include <string.h>

// Function to perform Vigenère encryption
void vigenereEncrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY";  // Replace with your desired key
    char message[] = "This is a secret message.";  // Replace with your message

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    return 0;
}

```
# DECRYPTION
```
#include <stdio.h>
#include <string.h>

// Function to perform Vigenère decryption
void vigenereDecrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY";  // Replace with your desired key
    char encryptedMessage[] = "family";  // Replace with your encrypted message

    // Decrypt the message
    vigenereDecrypt(encryptedMessage, key);
    printf("Decrypted Message: %s\n", encryptedMessage);

    return 0;
}

```

## OUTPUT:
# ENCRYPTION
![image](https://github.com/user-attachments/assets/e9857840-fdaf-4ab6-a649-5828e703cc7e)

# DECRYPTION
![image](https://github.com/user-attachments/assets/2ee46a5e-ba2c-4671-9dd4-08e7764bb89d)

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
# ENCRYPTION
```
#include <stdio.h>
#include <string.h>

void railFenceEncrypt(char *str, int rails, char *result) {
    int len = strlen(str);
    int code[rails][len];
    int i, j, count = 0;

    // Initialize the code array to zero
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }

    j = 0;  // To iterate over the characters of the string
    while (j < len) {
        if (count % 2 == 0) {
            // Moving down the rails
            for (i = 0; i < rails && j < len; i++) {
                code[i][j] = (int)str[j];
                j++;
            }
        } else {
            // Moving up the rails
            for (i = rails - 2; i > 0 && j < len; i--) {
                code[i][j] = (int)str[j];
                j++;
            }
        }
        count++;
    }

    // Collecting the encrypted message
    int k = 0;
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] != 0) {
                result[k++] = (char)code[i][j];
            }
        }
    }
    result[k] = '\0';  // Null-terminate the result string
}

int main() {
    char message[] = "HELLO WORLD";
    int rails = 3;
    char encrypted[1000];

    railFenceEncrypt(message, rails, encrypted);
    printf("Encrypted Message: %s\n", encrypted);

    return 0;
}

```
# DECRYTION
```
#include <stdio.h>
#include <string.h>

void railFenceDecrypt(char *str, int rails, char *result) {
    int len = strlen(str);
    int code[rails][len];
    int i, j, count = 0;

    // Initialize the code array to zero
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }

    j = 0;
    while (j < len) {
        if (count % 2 == 0) {
            // Marking positions down the rails
            for (i = 0; i < rails && j < len; i++) {
                code[i][j] = 1;
                j++;
            }
        } else {
            // Marking positions up the rails
            for (i = rails - 2; i > 0 && j < len; i--) {
                code[i][j] = 1;
                j++;
            }
        }
        count++;
    }

    // Fill the code array with the cipher text
    int k = 0;
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] == 1) {
                code[i][j] = (int)str[k++];
            }
        }
    }

    // Reconstructing the original message
    j = 0;
    count = 0;
    while (j < len) {
        if (count % 2 == 0) {
            for (i = 0; i < rails && j < len; i++) {
                result[j++] = (char)code[i][j];
            }
        } else {
            for (i = rails - 2; i > 0 && j < len; i--) {
                result[j++] = (char)code[i][j];
            }
        }
        count++;
    }
    result[len] = '\0';  // Null-terminate the result string
}

int main() {
    char encrypted[] = "HOLELWRDLO";
    int rails = 3;
    char decrypted[1000];

    railFenceDecrypt(encrypted, rails, decrypted);
    printf("Decrypted Message: %s\n", decrypted);

    return 0;
}

```

## OUTPUT:
# ENCRYPTION
![image](https://github.com/user-attachments/assets/be6b1b06-ff41-4a98-b1dc-4f1f6895875a)

# DECRYPTION
![image](https://github.com/user-attachments/assets/7b7edd67-1553-4072-bdb1-f663d056d481)

## RESULT:
The program is executed successfully.
