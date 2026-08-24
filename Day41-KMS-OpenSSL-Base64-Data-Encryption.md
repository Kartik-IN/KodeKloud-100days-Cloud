# ☁️ Day 41 – AWS KMS Encryption with OpenSSL Base64 Conversion

---

## 📌 Task Overview

This task demonstrates a common AWS encryption workflow where **AWS KMS** was used to perform the actual encryption and decryption, while **OpenSSL** was used as a fallback for Base64 conversion because the `base64` command was not installed on the AWS client.

The key learning point is that **AWS KMS handled the cryptographic operations**, while **OpenSSL simply converted Base64 text to binary data and vice versa**.

---

## 🧠 Root Cause

When the command below was run:

```bash
base64 -d /root/EncryptedData.base64 > /root/EncryptedData.bin
```

the system returned:

```text
base64: command not found
```

Because the `base64` utility was not available, OpenSSL was used instead.

---

## 🔐 Why OpenSSL?

OpenSSL supports Base64 encoding and decoding:

```bash
openssl base64 -d -in input -out output
```

So the equivalent command became:

```bash
openssl base64 -d \
  -in /root/EncryptedData.base64 \
  -out /root/EncryptedData.bin
```

This performs the same general job as:

```bash
base64 -d /root/EncryptedData.base64 > /root/EncryptedData.bin
```

---

## ⚠️ Important Distinction

### AWS KMS

AWS KMS was responsible for the actual encryption and decryption.

```text
SensitiveData.txt
       ↓
    AWS KMS
       ↓
Encrypted ciphertext
```

### OpenSSL

OpenSSL was used only for Base64 conversion.

```text
Base64 ciphertext
       ↓
    OpenSSL
       ↓
Binary ciphertext
EncryptedData.bin
```

Later, OpenSSL was also used to decode the Base64 plaintext returned by the AWS CLI:

```text
AWS KMS decrypt
       ↓
Base64 plaintext
       ↓
OpenSSL -d
       ↓
DecryptedData.txt
```

---

## 🧩 Summary of Tool Roles

| Tool | Purpose |
| --- | --- |
| AWS KMS | Actual encryption and decryption |
| OpenSSL | Base64 encode/decode fallback |
| `base64` | Normal Base64 tool, but not installed |

---

## 🛠️ Key Commands Used

### Decode Base64 with OpenSSL

```bash
openssl base64 -d \
  -in /root/EncryptedData.base64 \
  -out /root/EncryptedData.bin
```

### Later decode decrypted plaintext if Base64 output is returned

```bash
openssl base64 -d \
  -in /root/DecryptedPlaintext.base64 \
  -out /root/DecryptedData.txt
```

---

## 🧠 Key Learning

The key concept is that **AWS KMS handled the cryptographic task**, while **OpenSSL was simply a replacement for the missing `base64` command**.

In short:

```text
AWS KMS = real encryption/decryption
OpenSSL = Base64 conversion helper
```

This is a common issue in minimal Linux environments where `base64` may not be installed by default.

---

## ✅ Final Outcome

The team successfully completed the Base64 conversion workaround using OpenSSL, allowing the encrypted data to be processed correctly and verified after KMS decryption.

The workflow was:

```text
Sensitive file
   ↓
AWS KMS encrypt
   ↓
Base64 ciphertext
   ↓
OpenSSL decode
   ↓
Binary ciphertext
   ↓
AWS KMS decrypt
   ↓
Base64 plaintext
   ↓
OpenSSL decode
   ↓
Decrypted file
```

---

## 📚 AWS Concepts Practiced

- AWS KMS
- Encryption and decryption
- Base64 conversion
- OpenSSL fallback
- Secure data handling
- KMS plaintext/ciphertext workflow

---

## 📅 Cloud 100 Days Progress

**Day 41 – AWS KMS Encryption with OpenSSL Base64 Conversion**

Status: ✅ **Completed**
