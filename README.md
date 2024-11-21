# EX.NO-1-IMPLEMENTATION OF CAESAR CIPHER

# AIM:
To develop a simple C program to implement Caeser Cipher.
# ALGORITHM:
### Step 1:
Design of Caeser Cipher algorithnm 
### Step 2:
Implementation using C or pyhton code
### Step 3:
Testing algorithm with different key values. 
### STEP-4: 
Else subtract the key from the plain text.
### STEP-5: 
Display the cipher text obtained above.
## PROGRAM:
```python
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

![Screenshot 2024-08-30 143347](https://github.com/user-attachments/assets/952a89e6-42e1-4ed8-bc13-c4f6625d33e4)


## RESULT:
The program for caesar cipher is executed successfully.

---------------------------------







# EX.-NO-2-IMPLEMENTATION-OF-PLAYFAIR-CIPHER


# AIM:
To develop a simple C program to implement PlayFair Cipher.

# ALGORITHM:
### STEP-1: 
Read the plain text from the user.
### STEP-2: 
Read the keyword from the user.
### STEP-3: 
Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the
remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
### STEP-4: 
Group the plain text in pairs and match the corresponding corner letters by forming a
rectangular grid.
### STEP-5: 
Display the obtained cipher text.

## PROGRAM:
 ```python
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

![Screenshot 2024-08-30 143425](https://github.com/user-attachments/assets/64dc160e-e664-41da-b417-299b3607231e)


## RESULT:
The program for playfair cipher is executed successfully.






---------------------------


# EX.-NO-3-IMPLEMENTATION-OF-HILL-CIPHER

# AIM:
To develop a simple C program to implement Hill Cipher.

# ALGORITHM:
### STEP-1: 
Read the plain text and key from the user.
### STEP-2: 
Split the plain text into groups of length three.
### STEP-3: 
Arrange the keyword in a 3*3 matrix.
### STEP-4: 
Multiply the two matrices to obtain the cipher text of length three.
### STEP-5: 
Combine all these groups to get the complete cipher text.

# PROGRAM:
```python

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

![Screenshot 2024-08-30 143509](https://github.com/user-attachments/assets/81808210-102f-4674-96b8-07185e96b344)


## RESULT:

Thus the hill cipher substitution technique had been implemented successfully.
-------------------------------------------------


# EX.-NO-4-IMPLEMENTATION-OF-VIGENERECIPHER


# AIM:
To develop a simple C program to implement Vigenere Cipher.

# ALGORITHM:
### STEP-1: 
Arrange the alphabets in row and column of a 26*26 matrix.
### STEP-2: 
Circulate the alphabets in each row to position left such that the first letter is attached to
last.
### STEP-3: 
Repeat this process for all 26 rows and construct the final key matrix.
### STEP-4: 
The keyword and the plain text is read from the user.
### STEP-5: 
The characters in the keyword are repeated sequentially so as to match with that of the
plain text.
### STEP-6: 
Pick the first letter of the plain text and that of the keyword as the row indices and column
indices respectively.
### STEP-7: 
The junction character where these two meet forms the cipher character.
### STEP-8: 
Repeat the above steps to generate the entire cipher

# PROGRAM:

```python

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

![Screenshot 2024-08-30 143535](https://github.com/user-attachments/assets/2f8fca05-7da2-43df-95eb-290f59bcdcba)



## RESULT:
The program is executed successfully

-----------------------------------------------------------------------















# EX.-NO-5-A-IMPLEMENTATION-OF-RAILFENCE-CIPHER

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

# ALGORITHM:

In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an
imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the
message is written downwards again until the whole plaintext is written out. The message is then
read off in rows. 

## PROGRAM:
```python

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

![Screenshot 2024-08-30 143603](https://github.com/user-attachments/assets/ecb158d7-7f48-44f0-bac5-8da4d95fc7b9)


## RESULT:
The program is executed successfully
