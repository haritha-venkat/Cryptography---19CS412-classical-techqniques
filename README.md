# Cryptography---19CS412-classical-techqniques

# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
// Function to perform Caesar Cipher encryption
void caesarEncrypt(char *text, int key) {
 for (int i = 0; text[i] != '\0'; i++) {
 char c = text[i];
 // Check if the character is an uppercase letter
 if (c >= 'A' && c <= 'Z') {
 text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
 }
 // Check if the character is a lowercase letter
 else if (c >= 'a' && c <= 'z') {
 text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
 }
 // Ignore non-alphabetic characters
 }
}
// Function to perform Caesar Cipher decryption
void caesarDecrypt(char *text, int key) {
 // Decryption is the same as encryption with a negative key
 caesarEncrypt(text, -key);
}
int main() {
 char message[100]; // Declare a character array to store the message
 int key;
 printf("Enter the message to encrypt: ");
 fgets(message, sizeof(message), stdin); // Read input from the user
 printf("Enter the Caesar Cipher key (an integer): ");
 scanf("%d", &key); // Read the key from the user
 // Encrypt the message using the Caesar Cipher
 caesarEncrypt(message, key);
 printf("Encrypted Message: %s", message);
 // Decrypt the message back to the original
 caesarDecrypt(message, key);
 printf("Decrypted Message: %s", message);
 return 0;
}
```
## OUTPUT:
![Screenshot 2024-08-30 140054](https://github.com/user-attachments/assets/912ee58c-60ae-4dff-8ba4-5aea093f3ef8)


## RESULT:
The program is executed successfully

---------------------------------







# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
 ```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30
// Function to convert the string to lowercase
void toLowerCase(char plain[], int ps)
{
int i;
for (i = 0; i < ps; i++) {
if (plain[i] > 64 && plain[i] < 91)
plain[i] += 32;
}
}
// Function to remove all spaces in a string
int removeSpaces(char* plain, int ps)
{
int i, count = 0;
for (i = 0; i < ps; i++)
if (plain[i] != ' ')
plain[count++] = plain[i];
plain[count] = '\0';
return count;
}
// Function to generate the 5x5 key square
void generateKeyTable(char key[], int ks, char keyT[5][5])
{
int i, j, k, flag = 0, *dicty;
// a 26 character hashmap
// to store count of the alphabet
dicty = (int*)calloc(26, sizeof(int));
for (i = 0; i < ks; i++) {
if (key[i] != 'j')
dicty[key[i] - 97] = 2;
}
dicty['j' - 97] = 1;
i = 0;
j = 0;
for (k = 0; k < ks; k++) {
if (dicty[key[k] - 97] == 2) {
dicty[key[k] - 97] -= 1;
keyT[i][j] = key[k];
j++;
if (j == 5) {
i++;
j = 0;
}
}
}
for (k = 0; k < 26; k++) {
if (dicty[k] == 0) {
keyT[i][j] = (char)(k + 97);
j++;
if (j == 5) {
i++;
j = 0;
}
}
}
}
// Function to search for the characters of a digraph
// in the key square and return their position
void search(char keyT[5][5], char a, char b, int arr[])
{
int i, j;
if (a == 'j')
a = 'i';
else if (b == 'j')
b = 'i';
for (i = 0; i < 5; i++) {
for (j = 0; j < 5; j++) {
if (keyT[i][j] == a) {
arr[0] = i;
arr[1] = j;
}
else if (keyT[i][j] == b) {
arr[2] = i;
arr[3] = j;
}
}
}
}
// Function to find the modulus with 5
int mod5(int a)
{
return (a % 5);
}
// Function to make the plain text length to be even
int prepare(char str[], int ptrs)
{
if (ptrs % 2 != 0) {
str[ptrs++] = 'z';
str[ptrs] = '\0';
}
return ptrs;
}
// Function for performing the encryption
void encrypt(char str[], char keyT[5][5], int ps)
{
int i, a[4];
for (i = 0; i < ps; i += 2) {
search(keyT, str[i], str[i + 1], a);
if (a[0] == a[2]) {
str[i] = keyT[a[0]][mod5(a[1] + 1)];
str[i + 1] = keyT[a[0]][mod5(a[3] + 1)];
}
else if (a[1] == a[3]) {
str[i] = keyT[mod5(a[0] + 1)][a[1]];
str[i + 1] = keyT[mod5(a[2] + 1)][a[1]];
}
else {
str[i] = keyT[a[0]][a[3]];
str[i + 1] = keyT[a[2]][a[1]];
}
}
}
// Function to encrypt using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[])
{
char ps, ks, keyT[5][5];
// Key
ks = strlen(key);
ks = removeSpaces(key, ks);
toLowerCase(key, ks);
// Plaintext
ps = strlen(str);
toLowerCase(str, ps);
ps = removeSpaces(str, ps);
ps = prepare(str, ps);
generateKeyTable(key, ks, keyT);
encrypt(str, keyT, ps);
}
// Driver code
int main()
{
char str[SIZE], key[SIZE];
// Key to be encrypted
strcpy(key, "Monarchy");
printf("Key text: %s\n", key);
// Plaintext to be encrypted
strcpy(str, "instruments");
printf("Plain text: %s\n", str);
// encrypt using Playfair Cipher
encryptByPlayfairCipher(str, key);
printf("Cipher text: %s\n", str);
return 0;
}
```
## OUTPUT:
![Screenshot 2024-08-30 140331](https://github.com/user-attachments/assets/def45f91-1350-41d2-bafe-ed1c361acdb2)


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

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>  // Include the necessary header for toupper()

int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

void encode(char a, char b, char c, char ret[4]) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;
    
    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];
    
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
}

void decode(char a, char b, char c, char ret[4]) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;
    
    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];
    
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;
    
    strcpy(msg, "SecurityLaboratory");
    printf("Simulation of Hill Cipher\n");
    printf("Input message : %s\n", msg);
    
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }
    
    // Remove spaces
    n = strlen(msg) % 3;
    
    // Append padding text X
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }
    
    printf("Padded message : %s\n", msg);
    
    for (int i = 0; i < strlen(msg); i += 3) {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        char ret[4];
        encode(a, b, c, ret);
        strcat(enc, ret);
    }
    
    printf("Encoded message : %s\n", enc);
    
    for (int i = 0; i < strlen(enc); i += 3) {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        char ret[4];
        decode(a, b, c, ret);
        strcat(dec, ret);
    }
    
    printf("Decoded message : %s\n", dec);
    
    return 0;
}
```
## OUTPUT:

![Screenshot 2024-08-30 140432](https://github.com/user-attachments/assets/f60c8330-3692-4033-bf73-9d305c7c410d)


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

## PROGRAM:
```
#include <stdio.h>
#include <string.h>

// Function to perform Vigenere encryption
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

// Function to perform Vigenere decryption
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
    char message[] = "This is a secret message.";  // Replace with your message

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
```
## OUTPUT:
![Screenshot 2024-08-30 140525](https://github.com/user-attachments/assets/f3654fb5-94c4-46d6-b841-c28e70fb88bb)


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

## PROGRAM:
```
#include <stdio.h>
#include <string.h>

// Function to encrypt the plaintext using Rail Fence Cipher
void encryptRailFence(char *plaintext, int key) {
    int len = strlen(plaintext);
    char rail[key][len];
    memset(rail, '\n', sizeof(rail));  // Initialize the rail matrix with newline characters

    int row = 0, col = 0;
    int direction_down = 0;  // To check whether the direction is down or up

    // Fill the rail matrix
    for (int i = 0; i < len; i++) {
        // Place the current character in the rail
        rail[row][col++] = plaintext[i];

        // Check the direction and change it if necessary
        if (row == 0 || row == key - 1) {
            direction_down = !direction_down;
        }

        // Move to the next row depending on the direction
        direction_down ? row++ : row--;
    }

    // Read the rail matrix row-wise to form the ciphertext
    printf("Encrypted Text: ");
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] != '\n') {
                printf("%c", rail[i][j]);
            }
        }
    }
    printf("\n");
}

// Function to decrypt the ciphertext using Rail Fence Cipher
void decryptRailFence(char *ciphertext, int key) {
    int len = strlen(ciphertext);
    char rail[key][len];
    memset(rail, '\n', sizeof(rail));  // Initialize the rail matrix with newline characters

    int row = 0, col = 0;
    int direction_down = 0;

    // Mark the positions to be filled
    for (int i = 0; i < len; i++) {
        // Mark the current position
        rail[row][col++] = '*';

        // Check the direction and change it if necessary
        if (row == 0 || row == key - 1) {
            direction_down = !direction_down;
        }

        // Move to the next row depending on the direction
        direction_down ? row++ : row--;
    }

    // Fill the marked positions with the ciphertext characters
    int index = 0;
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] == '*' && index < len) {
                rail[i][j] = ciphertext[index++];
            }
        }
    }

    // Read the rail matrix in a zigzag manner to form the plaintext
    printf("Decrypted Text: ");
    row = 0, col = 0;
    direction_down = 0;
    for (int i = 0; i < len; i++) {
        printf("%c", rail[row][col++]);

        // Check the direction and change it if necessary
        if (row == 0 || row == key - 1) {
            direction_down = !direction_down;
        }

        // Move to the next row depending on the direction
        direction_down ? row++ : row--;
    }
    printf("\n");
}

int main() {
    char plaintext[100];
    int key;

    // Input the plaintext and the key
    printf("Enter the plaintext: ");
    scanf("%[^\n]%*c", plaintext);  // Read until newline character
    printf("Enter the key: ");
    scanf("%d", &key);

    // Encrypt the plaintext
    encryptRailFence(plaintext, key);

    // Input the ciphertext for decryption
    char ciphertext[100];
    printf("Enter the ciphertext to decrypt: ");
    scanf(" %[^\n]%*c", ciphertext);  // Read until newline character

    // Decrypt the ciphertext
    decryptRailFence(ciphertext, key);

    return 0;
}

```
## OUTPUT:

![Screenshot 2024-08-30 141027](https://github.com/user-attachments/assets/925058cc-b107-4ed4-a389-f8b47e887ad8)


## RESULT:
The program is executed successfully
