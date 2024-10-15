### Tugas Criptografi

Latihan :

Lakukan enkripsi Playfair Cihper pada plaintext:

GOOD BROOM SWEEP CLEAN
REDWOOD NATIONAL STATE PARK
JUNK FOOD AND HEALTH PROBLEMS

Jawaban :

Dengan kunci “TEKNIK INFORMATIKA”

kodingannya 
# Playfair Cipher Encryption

def generate_key_matrix(key):
    # Remove duplicates from the key
    key = "".join(dict.fromkeys(key.upper().replace("J", "I")))
    
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"  # Exclude 'J'
    matrix = []
    
    for char in key:
        if char not in matrix and char in alphabet:
            matrix.append(char)
    
    for char in alphabet:
        if char not in matrix:
            matrix.append(char)
    
    # Create a 5x5 matrix
    key_matrix = [matrix[i:i+5] for i in range(0, 25, 5)]
    return key_matrix

def process_text(text):
    text = text.upper().replace("J", "I").replace(" ", "")
    processed_text = []
    
    # Break into digraphs (pairs of 2 letters)
    i = 0
    while i < len(text):
        a = text[i]
        b = text[i+1] if i+1 < len(text) else 'X'
        if a == b:
            processed_text.append(a + 'X')
            i += 1
        else:
            processed_text.append(a + b)
            i += 2
    
    if len(processed_text[-1]) == 1:  # Add padding if needed
        processed_text[-1] += 'X'
    
    return processed_text

def find_position(char, matrix):
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col
    return None

def encrypt_digraph(digraph, matrix):
    a_row, a_col = find_position(digraph[0], matrix)
    b_row, b_col = find_position(digraph[1], matrix)
    
    if a_row == b_row:
        # Same row: shift to the right
        return matrix[a_row][(a_col + 1) % 5] + matrix[b_row][(b_col + 1) % 5]
    elif a_col == b_col:
        # Same column: shift down
        return matrix[(a_row + 1) % 5][a_col] + matrix[(b_row + 1) % 5][b_col]
    else:
        # Rectangle: swap columns
        return matrix[a_row][b_col] + matrix[b_row][a_col]

def encrypt_playfair(plaintext, key):
    key_matrix = generate_key_matrix(key)
    processed_text = process_text(plaintext)
    
    encrypted_text = ""
    for digraph in processed_text:
        encrypted_text += encrypt_digraph(digraph, key_matrix)
    
    return encrypted_text

# Example usage
key = "TEKNIK INFORMATIKA"
plaintext1 = "GOOD BROOM SWEEP CLEAN"
plaintext2 = "REDWOOD NATIONAL STATE PARK"
plaintext3 = "JUNK FOOD AND HEALTH PROBLEMS"

# Encrypt the plaintexts
encrypted1 = encrypt_playfair(plaintext1, key)
encrypted2 = encrypt_playfair(plaintext2, key)
encrypted3 = encrypt_playfair(plaintext3, key)

print("Plaintext 1:", plaintext1)
print("Encrypted 1:", encrypted1)
print("Plaintext 2:", plaintext2)
print("Encrypted 2:", encrypted2)
print("Plaintext 3:", plaintext3)
print("Encrypted 3:", encrypted3)

![Screenshot 2024-10-15 085338](https://github.com/user-attachments/assets/ac70c7ca-f23e-420b-b779-a2cfe7284391)



