# Cryptography and PKI: One-Way Hashing and Self-Signed Certificates

Three hands-on labs covering two distinct cryptographic concepts: calculating and comparing one-way hash values with HashCalc and HashMyFiles to verify file integrity, and creating/binding a self-signed SSL certificate in IIS to enable HTTPS on a test web server.

## Scenario
Hashing and certificates both fall under "cryptography" but answer different questions — integrity (has this changed?) versus encryption and identity (can this be secured, and who am I actually talking to?). These labs worked through both.

## What I did

### Exercise 1: One-way hashing with HashCalc
- Installed HashCalc and created a test text file with sample content
- Computed MD5, SHA1, RIPEMD160, and CRC32 hashes simultaneously for the file
- Modified the file's content slightly, recalculated, and observed every hash value change completely — demonstrating the avalanche effect that makes hashing useful for tamper detection

### Exercise 2: Bulk hashing with HashMyFiles
- Deliberately deselected SHA-256, SHA-512, and SHA-384 in the tool's hash-type options, leaving MD5, SHA1, and CRC32 active
- Ran it against an entire sample folder at once (Add Folder), returning hash values for every file in one pass
- Noted why: MD5/SHA1 are fast and fine for detecting accidental corruption, but are cryptographically broken (collision-vulnerable) and unsuitable for security-sensitive integrity verification against a deliberate adversary — SHA-2/SHA-3 are the right choice there

### Exercise 3: Creating and binding a self-signed certificate in IIS
- Confirmed the starting problem: browsing to a test site over HTTPS failed outright ("This site can't be reached") with no certificate in place
- In IIS Manager → Server Certificates → Create Self-Signed Certificate: generated a certificate (`LuxuryTreats`) stored in the Personal certificate store
- Added an HTTPS site binding: port 443 (auto-set), the site's IP, hostname (`www.luxurytreats.com`), and selected the new certificate
- Refreshed the site and reloaded over HTTPS — got the expected "Your connection is not private" browser warning, proceeded past it, and confirmed the site now loaded over an encrypted (but browser-unverified) HTTPS connection

## Key takeaways
- Hashing proves integrity, not identity or trustworthiness — a matching hash only confirms content hasn't changed, it says nothing about whether that content should be trusted in the first place
- A hash algorithm's output is specific to that algorithm — different algorithms never produce comparable values for the same input, and each is computed independently
- The avalanche effect (a tiny input change causing a completely different hash output) is the core property that makes hashing useful for tamper/corruption detection
- MD5 and SHA1 remain useful for detecting accidental corruption but are cryptographically broken (collision-vulnerable) and inappropriate for security-sensitive integrity checks against a deliberate adversary — SHA-2/SHA-3 are the modern standard for that
- A self-signed certificate provides real encryption but no identity verification, since no trusted third-party CA vouches for it — appropriate for self-controlled testing environments, inappropriate for a real public-facing service
- The browser's "not private" warning on a self-signed cert is the trust model working correctly, not a bug to route around

## Tools
HashCalc, HashMyFiles, Internet Information Services (IIS) Manager, Google Chrome

---
*Completed in an isolated training lab environment on provisioned Windows VMs.*
