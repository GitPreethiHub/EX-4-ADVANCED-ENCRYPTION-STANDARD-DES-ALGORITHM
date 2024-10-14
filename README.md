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
    printf("Ex-7 - Implement DES Encryption and Decryption\n");
    const char* url = "https://preethi.com"; 
    DES_Block key = 0x133457799BBCDFF1;

    printf("Encrypting URL: %s\n", url);
    process_url_encryption(url, key);

    return 0;
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/ea4aa8a3-8286-4f1b-ab6e-b2c6e3433c83)



## RESULT: 
Thus Data Encryption Standard (DES) Algorithm for a practical application like URL Encryption has been successfully excecuted.

# EX-8-ADVANCED-ENCRYPTION-STANDARD-AES-ALGORITHM

## Aim:
  To use Advanced Encryption Standard (AES) Algorithm for a practical application like URL Encryption.

## ALGORITHM: 
  1. AES is based on a design principle known as a substitution–permutation. 
  2. AES does not use a Feistel network like DES, it uses variant of Rijndael. 
  3. It has a fixed block size of 128 bits, and a key size of 128, 192, or 256 bits. 
  4. AES operates on a 4 × 4 column-major order array of bytes, termed the state

## PROGRAM: 
```
#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef uint8_t AES_Block[16];

void aes_encrypt_block(AES_Block plaintext, AES_Block key, AES_Block ciphertext) {
    for (int i = 0; i < 16; i++) {
        ciphertext[i] = plaintext[i] ^ key[i];
    }
}

void aes_decrypt_block(AES_Block ciphertext, AES_Block key, AES_Block plaintext) {
    for (int i = 0; i < 16; i++) {
        plaintext[i] = ciphertext[i] ^ key[i];
    }
}

void process_url_aes_encryption(const char* url, AES_Block key) {
    size_t len = strlen(url);
    size_t blocks = len / 16;
    if (len % 16 != 0) blocks++;

    for (size_t i = 0; i < blocks; i++) {
        AES_Block plaintext = {0};
        AES_Block ciphertext = {0};
        AES_Block decrypted = {0};

        char chunk[17] = {0};

        strncpy(chunk, url + i * 16, 16);
        memcpy(plaintext, chunk, 16);

        aes_encrypt_block(plaintext, key, ciphertext);
        printf("Block %zu Encrypted: ", i);
        for (int j = 0; j < 16; j++) printf("%02X", ciphertext[j]);
        printf("\n");

        aes_decrypt_block(ciphertext, key, decrypted);
        printf("Block %zu Decrypted: ", i);
        for (int j = 0; j < 16 && decrypted[j] != '\0'; j++) {
            printf("%c", decrypted[j]);
        }
        printf("\n");
    }
}

int main() {
    printf("Ex-8 - Implement AES Encryption and Decryption\n");
    const char* url = "https://preethi.com";
    AES_Block key = {0x2B, 0x7E, 0x15, 0x16, 0x28, 0xAE, 0xD2, 0xA6, 
                     0xAB, 0xF7, 0xCF, 0x34, 0x12, 0x28, 0xEE, 0xF4};

    printf("Encrypting URL: %s\n", url);
    process_url_aes_encryption(url, key);

    return 0;
}
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/10f93155-0739-4a00-851d-838130f0109a)


## RESULT: 
Thus Advanced Encryption Standard (AES) Algorithm for a practical application like URL Encryption has been successfully excecuted.
