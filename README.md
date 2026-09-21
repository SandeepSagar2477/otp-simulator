# One-Time Pad (OTP) Cryptography Simulator

An interactive, mathematically rigorous, single-page web application demonstrating the principles of **Claude Shannon's Perfect Secrecy**, bitwise XOR operations, key stream entropy, and the mathematical vulnerabilities of key reuse (Two-Time Pad Attacks).

---

## 🌟 Overview & Core Features

The **One-Time Pad (OTP)** is the only encryption technique mathematically proven to achieve **Perfect Secrecy** (Claude Shannon, 1949). When implemented correctly, an encrypted ciphertext yields zero information regarding the original plaintext to an attacker with infinite computing power.

### Key Capabilities

1. **Encryption & Decryption Studio**:
   - Live XOR encryption $C = btoa(P \oplus K)$ and decryption $P = atob(C) \oplus K$.
   - CSPRNG random key stream generator using `window.crypto.getRandomValues()`.
   - Real-time character-by-character XOR table matrix rendering ASCII and 8-bit binary conversions.
   - Dynamic key length validation ($|Key| \ge |Plaintext|$).

2. **Bitwise XOR Inspector & Logic Gate Visualizer**:
   - Interactive 8-bit XOR logic gate inspector showing bit transitions ($0 \oplus 0 = 0$, $0 \oplus 1 = 1$, $1 \oplus 0 = 1$, $1 \oplus 1 = 0$).
   - Step-by-step bitwise navigation.

3. **Key Reuse Vulnerability Lab (Two-Time Pad Attack)**:
   - Interactive demonstration of why reusing a key breaks secrecy ($C_1 \oplus C_2 = P_1 \oplus P_2$).
   - **Crib Dragging Tool**: Drag candidate words across the XOR of two ciphertexts to uncover hidden plaintexts.

4. **Algorithm Security Comparison Matrix**:
   - Comprehensive comparative analysis of OTP vs. AES-256, RSA-4096, and Caesar Cipher.
   - Claude Shannon's 4 Rules for Perfect Secrecy checklist.

---

## 📸 Screenshots Showcase

### 1. Encryption Studio
![OTP Encryption Studio](screenshots/encryption.png)

### 2. Decryption & Matrix Breakdown
![OTP Decryption & Matrix](screenshots/decryption.png)

### 3. Key Length Validation
![Key Length Validation](screenshots/key-length.png)

### 4. Key Reuse Vulnerability Lab (Crib Dragging)
![Key Reuse Lab](screenshots/key-reuse.png)

### 5. Security Comparison Matrix
![Security Comparison Matrix](screenshots/comparison.png)

---

## 📐 Mathematical Foundations

### 1. OTP Encryption Formula
Given a plaintext string $P = (p_1, p_2, \dots, p_n)$ and a truly random secret key stream $K = (k_1, k_2, \dots, k_n)$ of equal length:

$$C_i = P_i \oplus K_i$$

### 2. OTP Decryption Formula
Decryption performs the inverse XOR operation:

$$P_i = C_i \oplus K_i = (P_i \oplus K_i) \oplus K_i = P_i \oplus (K_i \oplus K_i) = P_i \oplus 0 = P_i$$

### 3. Information Secrecy Proof (Claude Shannon, 1949)
For any ciphertext $C$ and any possible plaintext message $P$:

$$P(\text{Plaintext} = P \mid \text{Ciphertext} = C) = P(\text{Plaintext} = P)$$

Since every candidate key $K$ is uniformly distributed and equally likely, a given ciphertext $C$ can decrypt into *every single possible message of length $n$* with equal probability.

### 4. Two-Time Pad Attack Mechanics ($C_1 \oplus C_2 = P_1 \oplus P_2$)
If the same key stream $K$ is used to encrypt two different messages $P_1$ and $P_2$:

$$C_1 = P_1 \oplus K$$
$$C_2 = P_2 \oplus K$$

XORing $C_1$ and $C_2$ eliminates the key:

$$C_1 \oplus C_2 = (P_1 \oplus K) \oplus (P_2 \oplus K) = P_1 \oplus P_2$$

---

## 💻 Code Architecture

```javascript
// CSPRNG Secure Key Generation
function generateSecureRandomKey(length) {
    const randomBytes = new Uint8Array(length);
    crypto.getRandomValues(randomBytes);
    
    let key = "";
    const charset = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*";
    
    for (let i = 0; i < length; i++) {
        key += charset[randomBytes[i] % charset.length];
    }
    return key;
}

// XOR Encryption Function
function encryptOTP(plaintext, key) {
    if (key.length < plaintext.length) {
        throw new Error("Key length must equal or exceed plaintext length");
    }
    
    let ciphertext = "";
    for (let i = 0; i < plaintext.length; i++) {
        const plaintextChar = plaintext.charCodeAt(i);
        const keyChar = key.charCodeAt(i);
        const ciphertextChar = plaintextChar ^ keyChar;
        ciphertext += String.fromCharCode(ciphertextChar);
    }
    
    return btoa(ciphertext); // Base64 encoding
}

// XOR Decryption Function
function decryptOTP(ciphertextBase64, key) {
    const decodedCiphertext = atob(ciphertextBase64);
    
    if (key.length < decodedCiphertext.length) {
        throw new Error("Key length must equal or exceed ciphertext length");
    }
    
    let plaintext = "";
    for (let i = 0; i < decodedCiphertext.length; i++) {
        const ciphertextChar = decodedCiphertext.charCodeAt(i);
        const keyChar = key.charCodeAt(i);
        const plaintextChar = ciphertextChar ^ keyChar;
        plaintext += String.fromCharCode(plaintextChar);
    }
    
    return plaintext;
}
```

---

## 🚀 Quick Start / Local Deployment

1. **Clone or Download the Project**:
   ```bash
   git clone https://github.com/your-username/otp-simulator.git
   cd otp-simulator
   ```

2. **Run Locally**:
   Open `index.html` directly in any modern web browser or serve via Python:
   ```bash
   python -m http.server 8080
   ```
   Navigate to `http://localhost:8080`.

3. **Deploy to GitHub Pages**:
   - Push repository to GitHub.
   - Go to **Settings** > **Pages** > Select `main` branch root folder.

---

## 📜 Technology Stack

- **Frontend**: HTML5, CSS3 (Vanilla Dark Mode with Glassmorphism, CSS Variables)
- **Scripting Engine**: ES6+ JavaScript (Vanilla, Zero Dependencies)
- **Randomness Source**: Web Crypto API (`window.crypto.getRandomValues`)
- **Encoding**: Native `btoa()` / `atob()` Base64 Encoding

---

## 🎓 Educational License

Distributed under the MIT License. See `LICENSE` for more information.
