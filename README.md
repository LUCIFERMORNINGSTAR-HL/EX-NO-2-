## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyTable[5][5];

// Function to remove duplicate letters from key and create key table
void createKeyTable(char key[]) {
    int seen[26] = {0}; // To mark used letters
    int row = 0, col = 0;

    for (int i = 0; key[i] != '\0'; i++) {
        char ch = toupper(key[i]);
        if (ch == 'J') ch = 'I';
        if (ch < 'A' || ch > 'Z') continue;

        if (!seen[ch - 'A']) {
            keyTable[row][col++] = ch;
            seen[ch - 'A'] = 1;
            if (col == 5) {
                col = 0;
                row++;
            }
        }
    }

    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue;
        if (!seen[ch - 'A']) {
            keyTable[row][col++] = ch;
            if (col == 5) {
                col = 0;
                row++;
            }
        }
    }
}

// Function to find position of a letter in key table
void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Encrypt function
void encryptPlayfair(char text[]) {
    int len = strlen(text);
    char cipher[100];
    int k = 0;

    // Prepare text in pairs
    for (int i = 0; i < len; i++) {
        char a = toupper(text[i]);
        if (a == 'J') a = 'I';
        if (a < 'A' || a > 'Z') continue;

        char b;
        if (i + 1 < len) {
            b = toupper(text[i + 1]);
            if (b == 'J') b = 'I';
        } else b = 'X';

        if (a == b) b = 'X';
        else i++;

        int r1, c1, r2, c2;
        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        if (r1 == r2) { // same row
            cipher[k++] = keyTable[r1][(c1 + 1) % 5];
            cipher[k++] = keyTable[r2][(c2 + 1) % 5];
        } else if (c1 == c2) { // same column
            cipher[k++] = keyTable[(r1 + 1) % 5][c1];
            cipher[k++] = keyTable[(r2 + 1) % 5][c2];
        } else { // rectangle
            cipher[k++] = keyTable[r1][c2];
            cipher[k++] = keyTable[r2][c1];
        }
    }
    cipher[k] = '\0';

    printf("Encrypted Text: %s\n", cipher);
}

int main() {
    char key[50], text[100];

    printf("Enter the keyword: ");
    scanf("%s", key);

    printf("Enter the plaintext: ");
    scanf("%s", text);

    createKeyTable(key);

    printf("\n5x5 Key Table:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            printf("%c ", keyTable[i][j]);
        }
        printf("\n");
    }

    encryptPlayfair(text);

    return 0;
}
```




Output:
<img width="1808" height="947" alt="Screenshot 2025-11-02 140818" src="https://github.com/user-attachments/assets/218e7ba9-7ed6-44d2-9c5a-b423c35ca6d8" />
