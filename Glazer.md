class AESCipher:
    def __init__(self, key: str):
        self.key = key.encode('utf-8')

    def encrypt(self, data: str) -> str:
        iv = os.urandom(16)  # Generate a random IV
        cipher = AES.new(self.key, AES.MODE_CBC, iv)
        encrypted_bytes = cipher.encrypt(pad(data.encode('utf-8'), AES.block_size))
        return base64.b64encode(iv + encrypted_bytes).decode('utf-8')

    def decrypt(self, encrypted_data: str) -> str:
        raw_data = base64.b64decode(encrypted_data)
        cipher = AES.new(self.key, AES.MODE_CBC, raw_data[:16])
        decrypted_bytes = unpad(cipher.decrypt(raw_data[16:]), AES.block_size)
        return decrypted_bytes.decode('utf-8')
