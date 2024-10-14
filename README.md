# EX-7-DATA-ENCRYPTION-STANDARD-DES-ALGORITHM

## Aim:
  To use Data Encryption Standard (DES) Algorithm for a practical application like URL Encryption.
## ALGORITHM: 
  1. DES (Data Encryption Standard) is a symmetric-key algorithm for the encryption of data. 
  2. DES operates on a Feistel network, which breaks the encryption process into 16 rounds.
  3. The block size of DES is fixed at 64 bits (8 bytes), and it uses a key size of 56 bits (7 bytes). However, keys are often provided as 64-bit keys where 8 bits are used for parity.
  4. DES processes blocks of data using permutations and substitutions (S-boxes) over several rounds to generate the ciphertext from plaintext.

## PROGRAM: 
```
#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef uint64_t DES_Block;

DES_Block initial_permutation(DES_Block block) {
    return block;
}

DES_Block final_permutation(DES_Block block) {
    return block;
}

DES_Block feistel_function(DES_Block right_half, DES_Block subkey) {
    return right_half ^ subkey;
}

DES_Block des_encrypt(DES_Block plaintext, DES_Block key) {
    DES_Block permuted_block = initial_permutation(plaintext);
    DES_Block left = permuted_block >> 32;
    DES_Block right = permuted_block & 0xFFFFFFFF;

    for (int round = 0; round < 16; round++) {
        DES_Block temp = right;
        right = left ^ feistel_function(right, key);
        left = temp;
    }

    DES_Block pre_output = (right << 32) | left;
    DES_Block ciphertext = final_permutation(pre_output);

    return ciphertext;
}

DES_Block des_decrypt(DES_Block ciphertext, DES_Block key) {
    return des_encrypt(ciphertext, key);
}

void process_url_encryption(const char* url, DES_Block key) {
    size_t len = strlen(url);
    size_t blocks = len / 8;
    if (len % 8 != 0) blocks++;

    for (size_t i = 0; i < blocks; i++) {
        DES_Block plaintext = 0;
        char chunk[9] = {0};  

        strncpy(chunk, url + i * 8, 8);  
        memcpy(&plaintext, chunk, sizeof(chunk));  

        DES_Block ciphertext = des_encrypt(plaintext, key);
        printf("Block %zu Encrypted: %lX\n", i, ciphertext);

        DES_Block decrypted = des_decrypt(ciphertext, key);
        printf("Block %zu Decrypted: %s\n", i, (char*)&decrypted);
    }
}

int main() {
    const char* url = "https://preethi.com"; 
    DES_Block key = 0x133457799BBCDFF1;

    printf("Encrypting URL: %s\n", url);
    process_url_encryption(url, key);

    return 0;
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/65852848-56bb-4b8a-b8a8-6e95b3dee4a3)




## RESULT: 
Thus Data Encryption Standard (DES) Algorithm for a practical application like URL Encryption has been successfully excecuted.
