# Challenge Overview
Đề bài cung cấp một đoạn mã Python mã hóa flag theo hai bước:

Mã hóa polyalphabetic dựa trên một key được sinh từ seed.
Mã hóa xor với một giá trị xor_key.
Cách mã hóa phụ thuộc vào timestamp chẵn/lẻ:

Timestamp chẵn:
key_seed là một số ngẫu nhiên trong khoảng [1, 1000].
xor_key cố định là 42.
Timestamp lẻ:
key_seed cố định là 42.
xor_key là một số ngẫu nhiên trong khoảng [1, 255].
Flag được chia thành hai phần và mã hóa riêng biệt:

Nửa đầu (flag_half1) và nửa sau (flag_half2) được mã hóa bằng các key_seed và xor_key khác nhau, sau đó được ghi vào file output.txt.
Nhiệm vụ: Giải mã flag đầy đủ mà không biết timestamp cụ thể.

# Phân tích mã hóa
```python
for _ in range(2):
        key_seed, xor_key = get_timestamp_based_keys()
        key = generate_key(key_seed)
        encrypted_half = polyalphabetic_encrypt(flag_half1 if len(encrypted_flags) == 0 else flag_half2, key)
        encrypted_half = xor_cipher(encrypted_half, xor_key)
        encrypted_flags.append(encrypted_half.hex())
        time.sleep(1)
```
Mã hóa được thực hiện như sau:
Sinh key_seed và xor_key từ timestamp.
Sinh key từ key_seed và thực hiện mã hóa polyalphabetic_encrypt.
Kết quả từ bước trên được mã hóa tiếp bằng xor_cipher.
# Lỗ hổng:
Đây là mấu chốt của bài crypto, key_seed và xor_key được lấy dựa trên timestamp chẵn hoặc lẻ, 
vậy nên ta chỉ cần bruteforce 2 trường hợp timestamp chẵn hoặc lẻ mà không cần biết chính xác giá trị của nó. 
Nhưng ta có 2 nửa flag, nên để cho chắc chắn, ứng với mỗi nửa flag, ta xét 2 trường hợp timestamp chẵn hoặc lẻ, tổng là 4 lần.

# Ý tưởng giải bài: 
Vì cách encrypt flag của bài này là xor khá easy nên mình sẽ không nói về cách decrypt nữa, ta chỉ cần tập 
trung vào Brute Force 4 Trường Hợp Timestamp vì có thể :

Xét tất cả các tổ hợp timestamp (chẵn/lẻ) cho flag_half1 và flag_half2:
Case 0: flag_half1 (chẵn), flag_half2 (chẵn).
Case 1: flag_half1 (lẻ), flag_half2 (lẻ).
Case 2: flag_half1 (chẵn), flag_half2 (lẻ).
Case 3: flag_half1 (lẻ), flag_half2 (chẵn).
Brute Force Keys:

Với timestamp chẵn: Brute force key_seed (1 đến 1000) và xor_key 42.
Với timestamp lẻ: Brute force xor_key (1 đến 255) và key_seed 42.
Kết Hợp Kết Quả:

Lọc nửa đầu (flag_half1) bắt đầu bằng "HTB".
Lọc nửa sau (flag_half2) kết thúc bằng "}".
Kết hợp mọi tổ hợp nửa đầu và nửa sau để tạo flag đầy đủ.

# Code
```python
import random
import string
import base64

# Hàm sinh key giống như trong code gốc
def generate_key(seed, length=16):
    random.seed(seed)
    key = ''.join(random.choice(string.ascii_letters + string.digits) for _ in range(length))
    return key

# Hàm mã hóa/giải mã polyalphabetic
def polyalphabetic_decrypt(ciphertext, key):
    ciphertext = base64.b64decode(ciphertext).decode()
    key_length = len(key)
    plaintext = []
    for i, char in enumerate(ciphertext):
        key_char = key[i % key_length]
        decrypted_char = chr((ord(char) - ord(key_char)) % 256)
        plaintext.append(decrypted_char)
    return ''.join(plaintext)

# Hàm XOR giải mã
def xor_cipher_decrypt(text, key):
    return bytes([c ^ key for c in text])

# Hàm giải mã flag
def decrypt_flag(encrypted_flag_hex, timestamp_even):
    
    encrypted_flag = bytes.fromhex(encrypted_flag_hex)  
    possible_flags = []

    if timestamp_even:
        # Timestamp chẵn: Brute force key_seed trước
        for key_seed in range(1, 1001):  
            key = generate_key(key_seed)
            xor_key = 42
            try:
                intermediate_flag = xor_cipher_decrypt(encrypted_flag, xor_key)
                decrypted_flag = polyalphabetic_decrypt(intermediate_flag, key)
                possible_flags.append(decrypted_flag)
            except Exception as e:
                continue
    else:
        for xor_key in range(1, 256):  # Brute force xor_key
            intermediate_flag = xor_cipher_decrypt(encrypted_flag, xor_key)     
            key_seed = 42
            key = generate_key(key_seed)
            try:
                decrypted_flag = polyalphabetic_decrypt(intermediate_flag, key)
                possible_flags.append(decrypted_flag)
            except Exception as e:
                continue

    return possible_flags


# Đọc dữ liệu từ file output.txt
with open('output.txt', 'r') as f:
    encrypted_flags = f.read().splitlines("\n")

# Brute force qua 4 trường hợp
final_flags = []

# Chạy 2 lần brute force cho flag1 và flag2 với cả timestamp chẵn/lẻ
possible_flag1_even = decrypt_flag(encrypted_flags[0], timestamp_even=True)
possible_flag1_odd = decrypt_flag(encrypted_flags[0], timestamp_even=False)
possible_flag2_even = decrypt_flag(encrypted_flags[1], timestamp_even=True)
possible_flag2_odd = decrypt_flag(encrypted_flags[1], timestamp_even=False)

# Lọc flag1 hợp lệ
valid_flag1_even = [flag for flag in possible_flag1_even if flag.startswith("HTB{")]
valid_flag1_odd = [flag for flag in possible_flag1_odd if flag.startswith("HTB{")]

# Lọc flag2 hợp lệ
valid_flag2_even = [flag for flag in possible_flag2_even if flag.endswith("}")]
valid_flag2_odd = [flag for flag in possible_flag2_odd if flag.endswith("}")]

# Kết hợp 4 trường hợp
for flag1 in valid_flag1_even:
    for flag2 in valid_flag2_even:
        final_flags.append(flag1 + flag2)
    for flag2 in valid_flag2_odd:
        final_flags.append(flag1 + flag2)

for flag1 in valid_flag1_odd:
    for flag2 in valid_flag2_even:
        final_flags.append(flag1 + flag2)
    for flag2 in valid_flag2_odd:
        final_flags.append(flag1 + flag2)

# In kết quả
for flag in final_flags:
    print(flag)
# HTB{timestamp_based_encryption_is_so_secure_i_promise}
```
