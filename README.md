Offline Text Encryptor - README

Overview

This is a client-side HTML/JavaScript encryption tool for securing sensitive text. All encryption happens in your browser—nothing is sent to a server. Use it to encrypt notes, passwords, or documents with a strong password.

Features

AES-256-GCM encryption: Enterprise-grade symmetric encryption
PBKDF2 key derivation: Password-based key generation with 100,000 iterations
Offline operation: No internet connection required
No server storage: All processing happens in your browser
Base64 encoded output: Encrypted text is portable and shareable
How It Works

Encryption Process

You provide a password and plaintext.
A random salt (16 bytes) is generated.
A random initialization vector (IV) (12 bytes) is generated.
Your password is hashed using PBKDF2 (SHA-256, 100,000 iterations) to derive a 256-bit key.
The plaintext is encrypted using AES-256-GCM.
The result is packaged as: [salt][IV][ciphertext] and Base64-encoded.
Decryption Process

You paste the Base64-encoded ciphertext and enter your password.
The salt and IV are extracted from the ciphertext.
Your password is hashed with the extracted salt using the same PBKDF2 parameters.
The ciphertext is decrypted using AES-256-GCM.
If the password is correct, plaintext is recovered; otherwise, decryption fails.
Ciphertext Format

The encrypted output follows this structure:



Base64([salt (16 bytes)][IV (12 bytes)][ciphertext + auth tag])
Salt: Random 16 bytes, used in key derivation
IV: Random 12 bytes, required for AES-GCM
Ciphertext + Auth Tag: Encrypted data with authentication tag
This format is consistent and reproducible—you can decrypt your data with any tool that implements the same algorithm and parameters.

Cryptographic Specifications

Parameter	Value
Encryption Algorithm	AES-256-GCM
Key Derivation	PBKDF2 with SHA-256
PBKDF2 Iterations	100,000
Key Length	256 bits (32 bytes)
IV Length	12 bytes
Salt Length	16 bytes
Authentication	GCM built-in authentication tag
Security Considerations

✅ Strong Points

AES-256-GCM is NIST-approved and unbreakable with current technology
PBKDF2 with 100,000 iterations makes brute-force attacks computationally expensive
Random salt and IV per encryption prevent rainbow table attacks and ensure identical plaintexts produce different ciphertexts
⚠️ Limitations

Password strength is the bottleneck
A weak password (e.g., "password123") can be cracked in minutes to hours
Use truly random passwords of 20+ characters for long-term security
Example crack times (1 billion guesses/sec with PBKDF2 slowdown):
10 random characters: ~30 years
15 random characters: ~500 million years
20 random characters: ~500 trillion years
32 random characters: ~10^41 years
Browser environment
The decryption key is exposed in browser memory during use
Page source code is visible (this is not a weakness—all components are standardized)
Use on trusted devices only
No built-in authentication
If decryption fails, you won't know if it's due to a wrong password or corrupted data
The ciphertext is not signed, so tampering isn't detected (GCM protects integrity, but doesn't authenticate the source)
Usage

Encryption

Open encryptor.html in a web browser
Enter your password in the "Password" field
Paste or type your plaintext in the "Text to Encrypt" field
Click Encrypt
Copy the Base64-encoded ciphertext from the output
Decryption

Open encryptor.html in a web browser
Enter your password in the "Password" field
Paste the Base64-encoded ciphertext in the "Encrypted Text" field
Click Decrypt
View the plaintext in the output
Password Recommendations

Use Case	Password Type	Example	Security
Temporary encryption	Memorable phrase	"MyDog#2024!Blue"	~500 years to crack
Long-term security	20 random chars	"aK9$mL2@pQ7!xJ5vN3"	~500 trillion years
Maximum security	32 random chars	"aK9$mL2@pQ7!xJ5vN3bW1#cZ4&dE6^fG"	~10^41 years
Generate random passwords with: openssl rand -base64 32 (Linux/Mac) or use a password manager like Bitwarden or 1Password.

Reconstructing This Tool

If you lose the HTML file, you can recreate the decryption tool using any cryptography library that supports:

AES-256-GCM
PBKDF2 (SHA-256, 100,000 iterations)
Base64 encoding/decoding
Example libraries:

JavaScript: TweetNaCl.js, libsodium.js, crypto-js
Python: cryptography library
Go: crypto/aes, crypto/pbkdf2
Ruby: OpenSSL module
Simply implement the same algorithm and ciphertext format documented above.

Backup & Storage

Recommended Storage for This HTML File

GitHub (Private Repository) — Free, version-controlled, accessible anywhere
Cloud Storage (Google Drive, Dropbox, AWS S3) — Convenient and backed up
Self-Hosted Server — Full control, requires maintenance
USB Drive — Offline backup, manual management
Do NOT Use Blockchain

Storing this HTML on blockchain is impractical (high cost, slow retrieval) and unnecessary (the file is not high-value data and doesn't require immutability).

Troubleshooting

Issue	Cause	Solution
Decryption fails	Wrong password OR corrupted ciphertext	Verify password spelling; ensure full Base64 string is copied
Output is gibberish	Password is wrong	Try again with correct password
Can't open file	Browser doesn't support Web Crypto API	Use a modern browser (Chrome, Firefox, Safari, Edge)
Slow encryption	PBKDF2 iterations (intentional slowdown)	This is normal; wait 1–2 seconds per operation
License & Disclaimer

This tool is provided as-is for educational and personal use. The author assumes no liability for data loss, security breaches, or misuse. Always:

Test encryption/decryption before storing important data
Back up encrypted files in multiple locations
Use strong, random passwords
Keep this HTML file secure
Technical Notes

Browser Compatibility: Requires Web Crypto API (all modern browsers)
File Size: ~5–20 KB depending on implementation
Dependencies: None (uses native browser crypto)
No Network: Entirely offline; no data leaves your device
Further Reading

NIST SP 800-38D: GCM Mode
RFC 2898: PBKDF2
AES Encryption Strength
Last Updated: September 2026
Version: 1.0
