# CHAOS — Dynamic Key Encryption Lab

A dependency-free browser project for exploring chaos-based dynamic key encryption. Open `dist/` through a local HTTP server (localhost) or deploy over HTTPS.

## Run locally

```sh
python3 -m http.server 5173 --directory dist
```

Then open http://localhost:5173.

## Included

- UTF-8 message encryption and authenticated decryption
- Fresh random 128-bit nonce per encryption
- PBKDF2-SHA-256 (100,000 iterations) derives 64 bytes from the passphrase
- Logistic map (r = 3.99), 1,000 warm-up iterations, two steps per byte
- XOR stream encryption with previous ciphertext feedback
- HMAC-SHA-256 verifies the version, nonce and ciphertext before decryption
- Ciphertext download and copy, editable ciphertext for independent decryption
- Actual Shannon entropy, byte distribution, bit balance, key avalanche, timing
- Inspectable initial state, key stream trajectory and first 16 byte transformations

The first 32 derived bytes drive the custom stream; the last 32 authenticate the package. The nonce also seeds the first ciphertext feedback byte. The ciphertext is a Base64-encoded JSON object containing version, nonce, raw ciphertext and tag. Downloaded files wrap this string in a `ciphertext` field; paste that field back into the lab to decrypt. Passphrases are never included in downloads or persisted.

This custom floating-point chaos cipher is educational and is not audited cryptography. Statistical tests do not establish security. Use a standard authenticated cipher such as AES-GCM for production secrets.

## Validate

```sh
node test.mjs
```
