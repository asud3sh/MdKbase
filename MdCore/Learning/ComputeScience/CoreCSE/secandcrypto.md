# 16 · SECURITY & CRYPTOGRAPHY

## 16.1 Foundations

### Security Basics

**Definition:** Security protects information and systems from unauthorized access, use, disclosure, disruption, modification, or destruction.

**Core concepts:**
- **Threat:** Potential cause of harm
- **Vulnerability:** Weakness that can be exploited
- **Risk:** Likelihood × impact
- **Exploit:** Attack that leverages vulnerability
- **Attack surface:** Sum of all vulnerabilities

**Threat actors:** Script kiddies, hacktivists, cybercriminals, nation-states, insiders.

**Recall:** *CIA + threat model.*

### Threat Modeling

**STRIDE:**
| Threat | Violates |
|--------|----------|
| Spoofing | Authentication |
| Tampering | Integrity |
| Repudiation | Non-repudiation |
| Information disclosure | Confidentiality |
| Denial of service | Availability |
| Elevation of privilege | Authorization |

**DREAD:** Damage, Reproducibility, Exploitability, Affected users, Discoverability.

**Attack trees:** Hierarchical decomposition of attack goals.

**Process:**
1. Identify assets
2. Identify adversaries
3. Identify attack surfaces
4. Enumerate threats
5. Mitigate

**Recall:** *Think like attacker.*

### CIA Triad

- **Confidentiality:** Data accessible only to authorized
- **Integrity:** Data unaltered
- **Availability:** Data accessible when needed

**Extensions:** Authenticity, non-repudiation, accountability.

**Recall:** *3 security goals.*

### Authentication

**Definition:** Verify identity.

**Factors:**
- **Something you know:** Password, PIN
- **Something you have:** Token, phone
- **Something you are:** Biometric
- **Somewhere you are:** Location
- **Something you do:** Behavior

**MFA:** Multi-factor authentication; combine factors.

**Protocols:** OAuth 2.0 (authorization), OpenID Connect (authentication), SAML (enterprise SSO).

**Password best practices:** Long, unique, hashed (bcrypt, Argon2), salted.

**Recall:** *Who are you?*

### Authorization

**Definition:** Determine what authenticated user can do.

**Models:**
- **RBAC:** Role-Based Access Control; roles → permissions
- **ABAC:** Attribute-Based Access Control; policies on attributes
- **ACL:** Access Control List; per-resource permissions
- **Capabilities:** Unforgeable tokens granting access

**Principle of least privilege:** Minimum necessary access.

**Recall:** *What can you do?*

---

## 16.2 Cryptography

### Cryptography

**Definition:** Mathematical techniques for secure communication.

**Goals:**
- **Confidentiality:** Encryption
- **Integrity:** Hash functions, MACs
- **Authenticity:** Digital signatures
- **Non-repudiation:** Signatures

**Types:**
- **Symmetric:** Same key for encrypt/decrypt
- **Asymmetric:** Public/private key pair
- **Hash:** One-way fingerprint

**Recall:** *Math for secrecy.*

### Symmetric Encryption

**Definition:** Same key encrypts and decrypts.

**Algorithms:**
- **AES:** Advanced Encryption Standard; 128/192/256-bit keys
- **ChaCha20:** Stream cipher; fast in software
- **3DES:** Legacy; deprecated

**Block vs stream:**
- **Block:** Encrypt fixed-size blocks (AES)
- **Stream:** Encrypt bit-by-bit (ChaCha20)

**Modes:**
| Mode | Properties |
|------|------------|
| ECB | Insecure; patterns visible |
| CBC | Chaining; needs IV |
| CTR | Counter; parallelizable |
| GCM | Authenticated encryption |

**Key management:** Generation, distribution, rotation, storage.

**Recall:** *One shared key.*

### Asymmetric Encryption

**Definition:** Public key encrypts; private key decrypts.

**Algorithms:**
- **RSA:** Based on factoring; 2048+ bits
- **ECC:** Elliptic curve; smaller keys
- **Diffie-Hellman:** Key exchange

**RSA example:**
1. Choose primes p, q
2. n = pq; φ(n) = (p−1)(q−1)
3. Choose e coprime to φ(n)
4. d = e⁻¹ mod φ(n)
5. Public: (e, n); Private: (d, n)
6. Encrypt: c = m^e mod n
7. Decrypt: m = c^d mod n

**ECC:** Based on discrete log on elliptic curves; 256-bit ECC ≈ 3072-bit RSA.

**Recall:** *Two keys.*

### Hash Functions

**Definition:** One-way function producing fixed-size digest.

**Properties:**
- **Preimage resistance:** Given h, hard to find m
- **Second preimage resistance:** Given m, hard to find m' with same hash
- **Collision resistance:** Hard to find any m, m' with same hash
- **Avalanche:** Small input change → large output change

**Algorithms:**
- **SHA-256:** 256-bit; widely used
- **SHA-3:** Keccak; sponge construction
- **BLAKE3:** Fast; parallelizable

**Uses:** Integrity, passwords, signatures, blockchain.

**Recall:** *One-way fingerprint.*

### Digital Signatures

**Definition:** Prove authenticity and integrity.

**Process:**
1. Signer hashes message
2. Signer encrypts hash with private key
3. Verifier decrypts with public key
4. Verifier compares hashes

**Algorithms:** RSA-PSS, ECDSA, EdDSA.

**Properties:** Authenticity, integrity, non-repudiation.

**Recall:** *Prove authenticity.*

### TLS

**Definition:** Transport Layer Security; secure channel over network.

**Handshake (TLS 1.3):**
1. Client hello (ciphers, key share)
2. Server hello (cert, key share)
3. Derive keys
4. Finished

**Certificates:** X.509; CA-signed; chain of trust.

**Forward secrecy:** Session keys not derivable from long-term key.

**Recall:** *Secure channel.*

### PKI (Public Key Infrastructure)

**Components:**
- **CA:** Certificate Authority; issues certs
- **RA:** Registration Authority; verifies identity
- **Certificate:** Binds public key to identity
- **CRL/OCSP:** Revocation

**Trust chain:** Root CA → intermediate → leaf.

**Recall:** *Trust infrastructure.*

### Key Management

**Lifecycle:** Generate → distribute → store → use → rotate → revoke → destroy.

**HSM:** Hardware Security Module; tamper-resistant.

**KMS:** Key Management Service; cloud-managed.

**Recall:** *Protect the keys.*

---

## 16.3 Application Security

### Web Security

**Same-origin policy:** Scripts can only access same origin (scheme + host + port).

**CSP (Content Security Policy):** Restrict sources of scripts, styles.

**CORS (Cross-Origin Resource Sharing):** Allow cross-origin requests.

**Cookies:** HttpOnly, Secure, SameSite.

**Recall:** *Browser security model.*

### OWASP Top 10

**2021 list:**
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable Components
7. Authentication Failures
8. Data Integrity Failures
9. Logging Failures
10. SSRF

**Recall:** *Top web risks.*

### Injection Attacks

**SQL injection:** `' OR '1'='1`
**Prevention:** Parameterized queries, ORM, input validation.

**Command injection:** `; rm -rf /`
**Prevention:** Avoid shell, validate input.

**LDAP injection:** Manipulate LDAP queries.

**Recall:** *Untrusted input = code.*

### XSS (Cross-Site Scripting)

**Types:**
- **Stored:** Malicious script stored on server
- **Reflected:** Script in URL reflected in response
- **DOM-based:** Client-side script manipulation

**Prevention:** Escape output, CSP, sanitize input.

**Recall:** *Inject script into page.*

### CSRF (Cross-Site Request Forgery)

**Definition:** Trick victim's browser into making request.

**Prevention:** CSRF tokens, SameSite cookies, origin checks.

**Recall:** *Trick browser into action.*

---

## 16.4 Systems Security

### Sandboxing

**Definition:** Isolate untrusted code.

**Techniques:** seccomp (syscall filtering), namespaces, WebAssembly, containers.

**Recall:** *Contain the damage.*

### Exploit Mitigation

**ASLR:** Address Space Layout Randomization; randomize memory layout.

**DEP:** Data Execution Prevention; mark memory non-executable.

**Stack canaries:** Detect stack overflow.

**CFI:** Control Flow Integrity; restrict indirect branches.

**Recall:** *Harden against exploits.*

### Malware

**Types:**
- **Virus:** Self-replicating; attaches to files
- **Worm:** Self-replicating; spreads over network
- **Trojan:** Disguised as legitimate
- **Ransomware:** Encrypts files; demands payment
- **Rootkit:** Hides presence
- **Spyware:** Steals information

**Recall:** *Malicious software types.*

### Reverse Engineering

**Definition:** Understand binary without source.

**Techniques:** Disassembly (IDA, Ghidra), decompilation, debugging (x64dbg, gdb).

**Recall:** *Understand binary.*

---

## 16.5 Advanced Topics

### Zero Trust

**Principle:** Never trust, always verify.

**Components:** Identity, device, network, application, data.

**Microsegmentation:** Fine-grained network isolation.

**Recall:** *No implicit trust.*

### Post-Quantum Cryptography

**Threat:** Quantum computers break RSA, ECC.

**Approaches:**
- **Lattice-based:** Kyber (KEM), Dilithium (signature)
- **Hash-based:** SPHINCS+
- **Code-based:** Classic McEliece

**NIST standards:** Kyber, Dilithium, Falcon, SPHINCS+.

**Recall:** *Quantum-safe crypto.*

### Zero-Knowledge Proofs

**Definition:** Prove knowledge without revealing it.

**Types:**
- **zk-SNARKs:** Succinct, non-interactive
- **zk-STARKs:** Transparent, scalable

**Use:** Privacy, blockchain, authentication.

**Recall:** *Prove without showing.*

### Differential Privacy

**Definition:** Add noise to protect individuals.

**Definition:** ε-differential privacy: P[M(D) ∈ S] ≤ e^ε × P[M(D') ∈ S] for adjacent datasets.

**Mechanisms:** Laplace, Gaussian.

**Use:** Census, ML, analytics.

**Recall:** *Privacy via noise.*

### Supply Chain Security

**Definition:** Secure dependencies and build process.

**Practices:** SBOM (Software Bill of Materials), sigstore, dependency scanning, reproducible builds.

**Recall:** *Trust your dependencies.*

**SOTA:** Post-quantum standards, confidential computing, homomorphic encryption, verifiable credentials.

---
