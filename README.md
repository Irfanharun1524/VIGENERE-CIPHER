# VIGENERE-CIPHER
## EX. NO: 4
 

## IMPLEMETATION OF VIGENERE CIPHER
 

## AIM:

To implement the Vigenere Cipher substitution technique using C program.

## DESCRIPTION:

To encrypt, a table of alphabets can be used, termed a tabula recta, Vigenère square,or Vigenère table. It consists of the alphabet written out 26 times in differnt rows, each
 
alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses adifferent alphabet from one of the rows. The alphabet used at each point repeating keyword.depends on a Each row starts with a key letter. The remainder of the row holds the letters A to Z. Although there are 26 key rows shown, you will only use as many keys as there are unique letters in the key string, here just 5 keys, {L, E, M, O, N}. For successive letters of the message, we are going to take successive letters of the key string, and encipher each message letter using its corresponding key row. Choose the next letter of the key, go along that row to find the column heading that	atches the message character; the letter at the intersection of
[key-row, msg-col] is the enciphered letter.


## ALGORITHM:

STEP-1: Arrange the alphabets in row and column of a 26*26 matrix.
STEP-2: Circulate the alphabets in each row to position left such that the first letter is attached to last.
STEP-3: Repeat this process for all 26 rows and construct the final key matrix.
STEP-4: The keyword and the plain text is read from the user.
STEP-5: The characters in the keyword are repeated sequentially so as to match with that of the plain text.
STEP-6: Pick the first letter of the plain text and that of the keyword as the row indices and column indices respectively.
STEP-7: The junction character where these two meet forms the cipher character.
STEP-8: Repeat the above steps to generate the entire cipher text.


## PROGRAM
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30

void toLower(char s[], int n) {
    for (int i = 0; i < n; i++)
        if (s[i] >= 'A' && s[i] <= 'Z') s[i] += 32;
}

int removeSpaces(char s[], int n) {
    int c = 0;
    for (int i = 0; i < n; i++)
        if (s[i] != ' ') s[c++] = s[i];
    s[c] = '\0'; return c;
}

int prepareText(char s[], int n) {
    if (n % 2 != 0) { s[n++] = 'z'; s[n] = '\0'; }
    return n;
}

void generateKeyTable(char key[], int n, char t[5][5]) {
    int used[26] = {0}, i = 0, j = 0;
    used['j' - 'a'] = 1;
    for (int k = 0; k < n; k++) if (key[k] != 'j') used[key[k] - 'a'] = 2;
    for (int k = 0; k < n; k++)
        if (used[key[k] - 'a'] == 2) {
            used[key[k] - 'a'] = 1;
            t[i][j++] = key[k];
            if (j == 5) { i++; j = 0; }
        }
    for (int k = 0; k < 26; k++)
        if (!used[k]) { t[i][j++] = k + 'a'; if (j == 5) { i++; j = 0; } }
}

void search(char t[5][5], char a, char b, int p[]) {
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';
    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++) {
            if (t[i][j] == a) { p[0] = i; p[1] = j; }
            if (t[i][j] == b) { p[2] = i; p[3] = j; }
        }
}

void encrypt(char txt[], char t[5][5], int n) {
    int p[4];
    for (int i = 0; i < n; i += 2) {
        search(t, txt[i], txt[i+1], p);
        if (p[0] == p[2]) {
            txt[i] = t[p[0]][(p[1]+1)%5];
            txt[i+1] = t[p[2]][(p[3]+1)%5];
        } else if (p[1] == p[3]) {
            txt[i] = t[(p[0]+1)%5][p[1]];
            txt[i+1] = t[(p[2]+1)%5][p[3]];
        } else {
            txt[i] = t[p[0]][p[3]];
            txt[i+1] = t[p[2]][p[1]];
        }
    }
}

void playfair(char txt[], char key[]) {
    int tlen = strlen(txt), klen = strlen(key);
    char table[5][5];
    toLower(key, klen); toLower(txt, tlen);
    klen = removeSpaces(key, klen);
    tlen = removeSpaces(txt, tlen);
    tlen = prepareText(txt, tlen);
    generateKeyTable(key, klen, table); 
    encrypt(txt, table, tlen);
}

int main() {
    char key[SIZE], text[SIZE];
    printf("Enter Key Text: "); scanf("%s", key);
    printf("Enter Plain Text: "); scanf("%s", text);
    playfair(text, key);
    printf("Cipher Text: %s\n", text);
    return 0;
}
```

## OUTPUT
<img width="1700" height="956" alt="Screenshot 2026-05-11 at 17-06-02 Online C Compiler - Programiz" src="https://github.com/user-attachments/assets/85412864-9213-4e42-8986-f4ade35af417" />


## RESULT
The program is executed successfully
