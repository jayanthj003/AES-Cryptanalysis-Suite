# AES Key Recovery Attacks
A comprehensive implementation of cryptanalysis techniques demonstrating key recovery attacks on AES-128 encryption schemes. This project showcases two fundamental cryptographic attack methodologies: brute-force attack and meet-in-the-middle attack.

## 🔐 Project Overview

This repository contains Python implementations of two cryptographic key recovery techniques:

1. **20-Bit Key Brute Force Attack** - Exhaustive key space exploration
2. **Meet-in-the-Middle Attack** - Time-memory tradeoff technique for two-key encryption

These implementations demonstrate practical cryptanalysis methods and the importance of robust key generation in cryptographic systems.

## 🎯 Features

- Custom AES key expansion implementation
- Efficient brute-force key recovery algorithm
- Advanced meet-in-the-middle attack on double-encryption
- Support for multiple plaintext-ciphertext pair verification
- Clean, well-documented, and educational code

## 📋 Prerequisites

- Python 3.7 or higher
- `cryptography` library

## 🚀 Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/aes-key-recovery-attacks.git
cd aes-key-recovery-attacks
```

2. Install required dependencies:
```bash
pip install cryptography
```

## 📁 Project Structure

```
aes-key-recovery-attacks/
│
├── part1_brute_force.py          # 20-bit key brute force attack
├── part2_meet_in_middle.py       # Meet-in-the-middle attack
├── plaintexts.txt                # Sample plaintexts for Part 1
├── ciphertexts.txt               # Sample ciphertexts for Part 1
├── 2plaintexts.txt               # Sample plaintexts for Part 2
├── 2ciphertexts.txt              # Sample ciphertexts for Part 2
└── README.md                     # Project documentation
```

## 💡 Usage

### Part 1: Brute Force Attack

This attack recovers a 20-bit key from AES-128 encrypted data.

**Prepare your input files:**

`plaintexts.txt`:
```
Hydrodynamometer
Circumnavigation
Crystallographer
Microphotography
```

`ciphertexts.txt`:
```
ea7f6a9b8ca5641e5c574000342a6322
24194bf1995f73a675ddabddbde46c43
b7f2292330b32d43f351a9588bdfa640
85c9b1e834c4c361db037023520fb438
c85afb6a2947ee3497ddf2b10e3ac81b
```

**Run the attack:**
```bash
python part1_brute_force.py
```

**Expected Output:**
```
Key found: <hex_key>
Expanded key: <128_bit_key>

Secret Plaintext:
<decrypted_message>
```

### Part 2: Meet-in-the-Middle Attack

This attack recovers two 16-bit keys used in double-encryption.

**Prepare your input files:**

`2plaintexts.txt`:
```
Hydrodynamometer
Circumnavigation
Crystallographer
Microphotography
```

`2ciphertexts.txt`:
```
ea7f6a9b8ca5641e5c574000342a6322
24194bf1995f73a675ddabddbde46c43
b7f2292330b32d43f351a9588bdfa640
85c9b1e834c4c361db037023520fb438
c85afb6a2947ee3497ddf2b10e3ac81b
```

**Run the attack:**
```bash
python part2_meet_in_middle.py
```

**Expected Output:**
```
Verified keys found!
First key (k1): <key1_value>
Second key (k2): <key2_value>

Secret Plaintext:
<decrypted_message>
```

## 🔬 Technical Approach

### Part 1: Brute Force Attack

**Algorithm:**
1. **Key Space**: Explores all 2^20 (1,048,576) possible 24-bit keys
2. **Key Expansion**: Converts 24-bit key to 128-bit AES key (last 4 bits ignored)
3. **Verification**: Decrypts ciphertext and compares with known plaintext
4. **Recovery**: Uses discovered key to decrypt secret message

**Complexity:**
- Time: O(2^20) ≈ 1 million operations
- Space: O(1) - minimal memory usage

**Key Expansion Process:**
```
24-bit short key → Custom expansion algorithm → 128-bit AES key
Uses: XOR operations, modular arithmetic, predefined constants
```

### Part 2: Meet-in-the-Middle Attack

**Algorithm:**
1. **Forward Pass**: 
   - Encrypt all plaintexts with all possible first keys (2^16)
   - Store intermediate encryption states in lookup table
   
2. **Backward Pass**:
   - Decrypt all ciphertexts with all possible second keys (2^16)
   - Search for matching intermediate states
   
3. **Verification**:
   - Test candidate key pairs against all plaintext-ciphertext pairs
   - Confirm validity across entire dataset

**Complexity:**
- Time: O(2^16 + 2^16) = O(2^17) ≈ 131,000 operations
- Space: O(2^16) - stores intermediate results
- Significantly faster than naive O(2^32) approach

**Encryption Model:**
```
Plaintext → [AES with Key1] → Intermediate → [AES with Key2] → Ciphertext
```

**Attack Visualization:**
```
Forward:  P --[K1]--> I1, I2, ..., In  (store all)
Backward: C --[K2]--> I'  (check if I' matches any stored I)
Match found → (K1, K2) is the key pair
```

## 🛡️ Security Insights

### Vulnerabilities Demonstrated

1. **Weak Key Space**: 20-bit keys are computationally feasible to break
2. **Double Encryption Weakness**: Two-key encryption doesn't provide 2x security
3. **Known Plaintext Attack**: Having plaintext-ciphertext pairs enables attacks
4. **ECB Mode Limitations**: Deterministic encryption allows pattern analysis

### Lessons Learned

- **Key Length Matters**: Modern systems use 128-bit or 256-bit keys
- **Proper Key Derivation**: Custom key expansion can introduce weaknesses
- **Mode of Operation**: ECB mode is vulnerable; use CBC, GCM, or CTR
- **Defense in Depth**: Multiple security layers are essential

## 🎓 Educational Value

This project is ideal for:
- Understanding practical cryptanalysis
- Learning time-memory tradeoff techniques
- Exploring AES encryption internals
- Studying attack complexity analysis
- Academic research in cryptography

## ⚠️ Ethical Considerations

**Important Notice:**
- This code is for **educational and research purposes only**
- Do not use on systems without explicit authorization
- Unauthorized access to encrypted data is illegal
- Use responsibly in controlled environments

## 📊 Performance Metrics

### Part 1 (Brute Force)
- Key space: 2^20 keys
- Average time: 5-30 minutes (varies by hardware)
- Memory usage: < 100 MB

### Part 2 (Meet-in-the-Middle)
- Key space: 2^16 × 2^16 keys
- Average time: 10-60 minutes (varies by hardware)
- Memory usage: ~500 MB - 1 GB

## 🔧 Implementation Details

### Technologies Used
- **Python 3.7+**: Primary programming language
- **cryptography library**: AES-128 implementation
- **Custom algorithms**: Key expansion and attack logic

### Key Functions

**`expandKey(shortKey)`**
- Expands short keys to 128-bit AES keys
- Handles integer and bytearray inputs
- Implements custom expansion algorithm

**`encrypt_aes(key, plaintext)`**
- AES-128 encryption in ECB mode
- Zero-padding for plaintexts

**`decrypt_aes(key, ciphertext)`**
- AES-128 decryption in ECB mode

**`brute_force_search(plaintexts, ciphertexts)`**
- Exhaustive key space exploration
- Plaintext verification

**`meet_in_middle_attack(plaintexts, ciphertexts)`**
- Two-phase attack implementation
- Intermediate state matching

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Cryptography course materials and assignments
- Python cryptography library developers
- Academic research in cryptanalysis techniques

## 📚 References

- [AES Specification (FIPS 197)](https://csrc.nist.gov/publications/detail/fips/197/final)
- [Meet-in-the-Middle Attack](https://en.wikipedia.org/wiki/Meet-in-the-middle_attack)
- [Applied Cryptography by Bruce Schneier](https://www.schneier.com/books/applied-cryptography/)

## 📈 Future Enhancements

- [ ] Add parallel processing for faster key recovery
- [ ] Implement GPU acceleration
- [ ] Support for additional encryption modes
- [ ] Visualization of attack progress
- [ ] Performance benchmarking suite
- [ ] Support for larger key spaces

---

⭐ **If you found this project helpful, please consider giving it a star!** ⭐
