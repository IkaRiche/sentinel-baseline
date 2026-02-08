# How to Verify a Receipt

\u003e **TL;DR**: Don't trust the bot. Trust the math. Here is how to verify any action receipt using standard tools.

---

## 1. Get the Public Key

Fetch the current authority public key (Ed25519):

```bash
curl -s https://authority.kilu.network/v1/public-key | jq
```

Response example:
```json
{
  "authority": "authority.kilu.network",
  "algorithm": "Ed25519",
  "key_id": "kilu-2026-02-08",
  "public_key": "MCowBQYDK2VwAyEA...",
  "fingerprint": "sha256:..."
}
```

## 2. Verify with OpenSSL

If you have a receipt signature (Base64) and the original intent content (JSON):

1. **Save the public key**:
   ```bash
   echo "MCowBQYDK2VwAyEA..." | base64 -d > public_key.pem
   ```

2. **Save the signature**:
   ```bash
   echo "SIGNATURE_BASE64_HERE" | base64 -d > signature.bin
   ```

3. **Verify**:
   ```bash
   echo -n '{"intent":"..."}' > intent.json
   openssl pkeyutl -verify -pubin -inkey public_key.pem -rawin -in intent.json -sigfile signature.bin
   ```

Output should be: `Signature Verified Successfully`

## 3. Verify with Node.js

```javascript
const crypto = require('crypto');

// 1. Get Key
const publicKey = crypto.createPublicKey({
  key: Buffer.from("MCowBQYDK2VwAyEA...", 'base64'),
  format: 'der',
  type: 'spki'
});

// 2. Data & Signature
const data = Buffer.from('{"intent":"..."}'); // The exact intent string
const signature = Buffer.from('SIGNATURE_BASE64...', 'base64');

// 3. Verify
const isVerified = crypto.verify(null, data, publicKey, signature);
console.log(isVerified ? "✅ Valid Receipt" : "❌ Invalid");
```
