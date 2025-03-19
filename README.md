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
PROGRAM:
```
CaearCipher.
def caesar_cipher(text, key, mode='encrypt'):
    result = ""
    
    for char in text:
        if char.isalpha():  # Check if the character is a letter
            shift = key if mode == 'encrypt' else -key
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base + shift) % 26 + base)
        else:
            result += char  # Keep non-alphabet characters unchanged
    
    return result

# Example usage
message =input("Enter Plain Text please:")
key = int(input("Enter the Key value please:"))  # Single key value

encrypted_text = caesar_cipher(message, key, mode='encrypt')
decrypted_text = caesar_cipher(encrypted_text, key, mode='decrypt')

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)
```

## OUTPUT:

Simulating Caesar Cipher.....

```
Input : hello
Encrypted Message : khoor
Decrypted Message : hello
```
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
```
import itertools

def generate_playfair_matrix(key):
    key = key.upper().replace("J", "I")  # Convert key to uppercase and replace J with I
    key = "".join(dict.fromkeys(key))  # Remove duplicate letters
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    key += "".join(c for c in alphabet if c not in key)
    
    matrix = [list(key[i:i+5]) for i in range(0, 25, 5)]
    return matrix

def find_position(matrix, letter):
    for row, line in enumerate(matrix):
        if letter in line:
            return row, line.index(letter)
    return None

def playfair_cipher(text, key, mode='encrypt'):
    matrix = generate_playfair_matrix(key)
    text = text.upper().replace("J", "I").replace(" ", "")  # Convert text to uppercase
    pairs = []
    i = 0

    while i < len(text):
        a = text[i]
        if i + 1 < len(text) and text[i] != text[i + 1]:
            b = text[i + 1]
            i += 2
        else:
            b = 'X'  # Add 'X' if the letter is repeated or if it's a single letter at the end
            i += 1

        pairs.append((a, b))

    result = ""
    for a, b in pairs:
        pos_a = find_position(matrix, a)
        pos_b = find_position(matrix, b)

        if pos_a is None or pos_b is None:
            continue  # Skip if letter is not found (shouldn't happen)

        row_a, col_a = pos_a
        row_b, col_b = pos_b

        if row_a == row_b:  # Same row
            col_a = (col_a + 1) % 5 if mode == 'encrypt' else (col_a - 1) % 5
            col_b = (col_b + 1) % 5 if mode == 'encrypt' else (col_b - 1) % 5
        elif col_a == col_b:  # Same column
            row_a = (row_a + 1) % 5 if mode == 'encrypt' else (row_a - 1) % 5
            row_b = (row_b + 1) % 5 if mode == 'encrypt' else (row_b - 1) % 5
        else:  # Rectangle swap
            col_a, col_b = col_b, col_a

        result += matrix[row_a][col_a] + matrix[row_b][col_b]

    return result

# Get user input
message = input("Enter the message: ").strip()
key = input("Enter the key: ").strip()

encrypted_text = playfair_cipher(message, key, mode='encrypt')
decrypted_text = playfair_cipher(encrypted_text, key, mode='decrypt')

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```
## OUTPUT:
```
Key text:    computer
Plain text:  communication
Cipher text: OMRMPCSGPTBDML
```
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
PROGRAM:
```

import numpy as np
from sympy import Matrix

def mod_inverse(matrix, mod):
    det = int(round(np.linalg.det(matrix))) % mod
    det_inv = pow(det, -1, mod)  # Modular inverse of determinant
    adjugate = Matrix(matrix).adjugate()
    return (det_inv * np.array(adjugate) % mod).astype(int)

def text_to_matrix(text, size):
    text = text.upper().replace(" ", "")
    while len(text) % size != 0:
        text += 'X'
    values = [ord(char) - ord('A') for char in text]
    return np.array(values).reshape(-1, size)

def matrix_to_text(matrix):
    return "".join(chr(int(round(num)) % 26 + ord('A')) for num in matrix.flatten())

def hill_cipher_encrypt(text, key_matrix):
    text_matrix = text_to_matrix(text, key_matrix.shape[0])
    encrypted_matrix = np.dot(text_matrix, key_matrix) % 26
    return matrix_to_text(encrypted_matrix)

def hill_cipher_decrypt(ciphertext, key_matrix):
    try:
        key_inverse = mod_inverse(key_matrix, 26)
    except ValueError:
        return "Error: Key matrix is not invertible mod 26. Choose a different key."
    cipher_matrix = text_to_matrix(ciphertext, key_matrix.shape[0])
    decrypted_matrix = np.dot(cipher_matrix, key_inverse) % 26
    return matrix_to_text(decrypted_matrix)

# Get user input
message = input("Enter the message: ").strip()
key_values = list(map(int, input("Enter the key matrix values (space-separated): ").split()))
size = int(len(key_values) ** 0.5)
key_matrix = np.array(key_values).reshape(size, size)

encrypted_text = hill_cipher_encrypt(message, key_matrix)
decrypted_text = hill_cipher_decrypt(encrypted_text, key_matrix)

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```

## OUTPUT:

Simulating Hill Cipher.....

```
Input Message     : hello
Encrypted Message : DPDKKB
Decrypted Message : HELLOX
```
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
PROGRAM:
```

def vigenere_cipher(text, key, mode='encrypt'):
    text = text.upper().replace(" ", "")
    key = key.upper()
    key_repeated = (key * (len(text) // len(key) + 1))[:len(text)]
    result = ""
    
    for t, k in zip(text, key_repeated):
        shift = ord(k) - ord('A')
        if mode == 'encrypt':
            result += chr((ord(t) - ord('A') + shift) % 26 + ord('A'))
        else:
            result += chr((ord(t) - ord('A') - shift) % 26 + ord('A'))
    
    return result

# Get user input
message = input("Enter the message: ").strip()
key = input("Enter the key: ").strip()

encrypted_text = vigenere_cipher(message, key, mode='encrypt')
decrypted_text = vigenere_cipher(encrypted_text, key, mode='decrypt')

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```
## OUTPUT:

Simulating Vigenere Cipher.....

```

Input Message     : HELLO
Encrypted Message : RIJVS
Decrypted Message : HELLO

```
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

PROGRAM:
```

def rail_fence_encrypt(text, rails):
    fence = [[] for _ in range(rails)]
    rail = 0
    direction = 1
    
    for char in text:
        fence[rail].append(char)
        rail += direction
        if rail == 0 or rail == rails - 1:
            direction *= -1
    
    return "".join("".join(row) for row in fence)

def rail_fence_decrypt(ciphertext, rails):
    fence = [['' for _ in range(len(ciphertext))] for _ in range(rails)]
    rail = 0
    direction = 1
    
    for i in range(len(ciphertext)):
        fence[rail][i] = '*'
        rail += direction
        if rail == 0 or rail == rails - 1:
            direction *= -1
    
    index = 0
    for row in range(rails):
        for col in range(len(ciphertext)):
            if fence[row][col] == '*' and index < len(ciphertext):
                fence[row][col] = ciphertext[index]
                index += 1
    
    result = []
    rail = 0
    direction = 1
    for i in range(len(ciphertext)):
        result.append(fence[rail][i])
        rail += direction
        if rail == 0 or rail == rails - 1:
            direction *= -1
    
    return "".join(result)

# Get user input
message = input("Enter the message: ").replace(" ", "")
rails = int(input("Enter the number of rails: "))

encrypted_text = rail_fence_encrypt(message, rails)
decrypted_text = rail_fence_decrypt(encrypted_text, rails)

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```
## OUTPUT:
Simulating Rail Fence.....

```

Enter a Secret Message : HELLO
Enter number of rails  : 3
Decrypted Message      : HOELL

```

## RESULT:
The program is executed successfully
