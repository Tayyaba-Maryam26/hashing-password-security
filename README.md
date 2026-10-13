# 🔐 Hashing & Password Security — Cybersecurity Learning Notes

Welcome to my learning notes on **Hashing and Password Security**. In this stage of my cybersecurity journey, I studied how hash functions work, different hash algorithms, modern password-hashing methods, salting, peppering, and the difference between online and offline password attacks.

My goal is not just to learn commands and tools, but to understand the concepts behind them and how they help protect digital systems.

---

## 📌 Table of Contents

1. [What Is a Hash?](#1--what-is-a-hash)
2. [Types of Hash Algorithms](#2--types-of-hash-algorithms)
3. [Modern Password-Hashing Methods](#3--modern-password-hashing-methods)
4. [Salting and Peppering](#4--salting-and-peppering)
5. [Online vs. Offline Password Attacks](#5--online-vs-offline-password-attacks)
6. [The Big Picture](#6--the-big-picture--how-everything-connects)
7. [Security Best Practices](#7--security-best-practices)
8. [Quick Revision](#8--quick-revision)
9. [Learning Resources](#9--learning-resources)

---

# 1. 🔑 What Is a Hash?

A **hash** is a fixed-size output generated when input data is processed by a hash function.

A hash function can process different kinds of data, such as text, files, or other digital information, and produce a digest.

### How Hashing Works

`Input Data → Hash Function → Hash Output`

For example:

- **Input:** `Hello World`
- **Algorithm:** SHA-256
- **Output:** A 256-bit hash, normally displayed as 64 hexadecimal characters.

The exact output depends on the input and the algorithm being used.

### Key Properties of Hash Functions

**1. Fixed-Length Output**

A hash algorithm produces an output of a predetermined size, regardless of the input's length. For example, SHA-256 always produces 256 bits.

**2. One-Way Property**

A cryptographic hash is designed to make it computationally infeasible to recover the original input from the hash alone. However, attackers may guess possible inputs and check whether they match.

**3. Deterministic**

The same input processed using the same algorithm produces the same output.

**4. Avalanche Effect**

A small change in the input should cause a significant, unpredictable-looking change in the output.

**5. Collision Resistance**

A collision occurs when two different inputs produce the same hash. Secure cryptographic hash functions are designed to make finding such collisions computationally infeasible.

### Hashing vs. Encryption

- **Hashing:** A one-way process; it is not designed to be reversed through decryption.
- **Encryption:** A reversible process when the correct decryption key is available.

A hash is like a digital fingerprint of data, not an encryption key.

### 📸 Poster 1 — What Is a Hash?

![Poster 1 — What Is a Hash?](posters/poster-1-hashing-basics.png)

---

# 2. 🧩 Types of Hash Algorithms

Different hash algorithms have different output sizes, designs, and security properties.

## MD5

- Produces a 128-bit hash.
- Usually displayed as 32 hexadecimal characters.
- No longer considered secure for collision-resistant security applications.
- Should not be used for new security-sensitive designs.

## SHA-1

- SHA stands for **Secure Hash Algorithm**.
- Produces a 160-bit hash, usually displayed as 40 hexadecimal characters.
- Has known collision weaknesses.
- Deprecated for security-sensitive uses that require collision resistance.

## SHA-2 Family

SHA-2 is a family of cryptographic hash algorithms that includes:

| Algorithm | Output Size | Hexadecimal Characters |
|---|---:|---:|
| SHA-224 | 224 bits | 56 |
| SHA-256 | 256 bits | 64 |
| SHA-384 | 384 bits | 96 |
| SHA-512 | 512 bits | 128 |

SHA-256 is widely used for data integrity checks and as part of many cryptographic systems.

## SHA-3 Family

SHA-3 is another family of cryptographic hash algorithms based on the Keccak design. It uses a different internal construction from SHA-2.

Its common variants include:

- SHA3-224
- SHA3-256
- SHA3-384
- SHA3-512

### Important Understanding

The hash output size depends on the selected algorithm, not on the input's length.

For example, SHA-256 produces a 256-bit output whether the input is a single word or a large file.

**Remember:** SHA-256 is useful for many cryptographic purposes, but it should not be used alone to store user passwords because it is designed to be fast.

### 📸 Poster 2 — Types of Hash Algorithms

![Poster 2 — Types of Hash Algorithms](posters/poster-2-hash-algorithms.png)

---

# 3. 🛡️ Modern Password-Hashing Methods

Password storage requires a different approach from ordinary data hashing.

A website should not store passwords in plain text. Instead, it should store password-verification data generated using an appropriate password-hashing method.

During login, the system processes the submitted password using the stored salt and parameters, then checks whether the result matches the saved record.

Password-hashing methods are designed to make password guessing more expensive.

## Argon2id

- A modern password-hashing option.
- Uses configurable memory and time costs.
- Makes large-scale password guessing more expensive.
- A strong choice for new systems when implemented with appropriate settings.

## bcrypt

- A widely used password-hashing method.
- Has an adjustable computational cost.
- Increasing the cost makes each password guess more expensive.

## scrypt

- Designed to be memory-intensive.
- Increases the resources required to test large numbers of password guesses.

## PBKDF2

- A password-based key-derivation method.
- Repeats cryptographic operations for a configurable number of iterations.
- Used in password storage and other key-derivation applications.

### SHA-256 vs. Password-Hashing Methods

SHA-256 is fast and useful for integrity checking. However, if an attacker obtains password hashes, a fast algorithm can allow many guesses to be tested quickly.

Methods such as Argon2id, bcrypt, scrypt, and PBKDF2 are designed to make each guess more expensive when configured appropriately.

These methods are not simply newer SHA algorithms; they are dedicated password-hashing or key-derivation approaches.

### 📸 Poster 3 — Password-Hashing Methods

![Poster 3 — Password-Hashing Methods](posters/poster-3-password-hashing-methods.png)

---

# 4. 🧂 Salting and Peppering

Salting and peppering can add protection to password-storage systems, but they work differently.

## What Is a Salt?

A **salt** is a random value generated for a password record and used as part of the password-hashing process.

The salt is normally stored alongside the resulting hash and its parameters.

### Why Is Salting Important?

- **Unique password records:** The same password can produce different stored results when different salts are used.
- **Protection against precomputed tables:** Salts make precomputed password-hash tables much less useful across many accounts.
- **No need for secrecy:** A salt can be stored with the hash because it is not intended to be a secret key.

A salt does not automatically make a weak password strong. It should be combined with an appropriate password-hashing method.

## What Is a Pepper?

A **pepper** is an additional secret value used in a password-storage design.

Unlike a salt, a pepper is kept separately from the password-hash database, such as in a protected secrets manager or application configuration.

### Why Is Peppering Important?

- Adds another layer of protection.
- Can help if the password-hash database is exposed but the pepper remains secret.
- Must be securely stored and carefully managed.

If the pepper is exposed along with the database, its additional protection may be lost.

## Salt vs. Pepper

| Feature | Salt | Pepper |
|---|---|---|
| Purpose | Helps protect against reusable precomputed tables and identical stored hashes | Adds an additional secret |
| Unique per password record | Normally yes | Depends on the system's design |
| Stored with the hash | Yes | No; stored separately |
| Must remain secret | No | Yes |

**Key takeaway:** A salt is public information stored with the hash. A pepper is a secret that must be protected separately.

### 📸 Poster 4 — Salting and Peppering

![Poster 4 — Salting and Peppering](posters/poster-4-salting-and-peppering.png)

---

# 5. 🌐 Online vs. Offline Password Attacks

Both online and offline attacks may use a list of password guesses. The key difference is **where the guesses are checked**.

## Online Password Attack

In an online attack, an attacker submits password guesses to a live website or authentication service.

### How It Works

1. The attacker has or creates a list of possible passwords.
2. Password guesses are submitted to the target's login system.
3. The website checks each attempt.
4. The system may detect repeated failures, delay attempts, or block further attempts.

### Common Defenses

- Rate limiting.
- Login delays and appropriate account protections.
- Multi-factor authentication (MFA).
- Monitoring and alerts for suspicious login attempts.

Online guessing interacts with the real login system, so its security controls can restrict or detect attempts.

## Offline Password Attack

An offline attack of this type occurs when an attacker has obtained a copy of password hashes or equivalent password-verification data.

Instead of submitting every guess to the website, the attacker tests candidates locally on their own computer.

### How It Works

1. The attacker obtains password-hash data.
2. They generate or collect possible password guesses.
3. A suitable password-auditing tool tests candidates against the obtained hashes.
4. If a candidate matches, the attacker may have identified the original password.

For example, John the Ripper can test password candidates against supported hashes in authorized auditing environments.

The tool does not decrypt the hash. It tests guesses and checks whether the results match the stored verification data.

If a password is recovered, attempting to use it on a live website is a separate online action. Access is not guaranteed because MFA, changed passwords, and other protections may still prevent login.

### Why Can Offline Attacks Be Dangerous?

- The website does not receive each local guess.
- Website login rate limits do not directly slow the local guessing process.
- The attacker can continue testing candidates locally, depending on available computing resources and the hashing method.

Success depends on password strength, the algorithm, its settings, and the attacker's resources.

## Online vs. Offline Comparison

| Feature | Online Attack | Offline Attack |
|---|---|---|
| Where guesses are checked | Live website or authentication service | Locally against obtained hash data |
| Does each guess contact the website? | Yes | No |
| Is hash data required? | Not for ordinary login guessing | Yes, for this type of offline hash attack |
| Do website rate limits directly restrict each guess? | They can | No |
| Relevant defenses | Rate limiting, MFA, monitoring | Strong password hashing, unique salts, strong passwords, database protection |

### 📸 Poster 5 — Online vs. Offline Attacks

![Poster 5 — Online vs. Offline Attacks](posters/poster-5-online-vs-offline.png)

---

# 6. 🗺️ The Big Picture — How Everything Connects

All these concepts work together to explain how passwords can be stored and protected.

### Step 1: A User Creates a Password

A user chooses a password to protect their account.

### Step 2: The Password Is Processed Securely

The system uses an appropriate password-hashing method, such as Argon2id or bcrypt, along with a salt and suitable parameters. A pepper may also be used as an additional secret.

### Step 3: The System Stores Verification Data

The website stores the password-hash record, salt, and relevant parameters—not the plaintext password.

### Step 4: The User Logs In

When the user submits a password, the system processes it using the stored settings and verifies whether it matches the saved record.

### Step 5: Attackers May Try to Guess Passwords

- **Online:** Guesses are submitted to the live login system.
- **Offline:** Guesses are tested locally if password-hash data has been obtained.

### Step 6: Strong Defenses Reduce Risk

Strong, unique passwords; suitable password-hashing methods; unique salts; securely managed peppers; MFA; and login protections all contribute to better security.

### 📸 Poster 6 — The Big Picture

![Poster 6 — The Big Picture](posters/poster-6-big-picture.png)

---

# 7. ✅ Security Best Practices

1. Never store passwords in plain text.
2. Avoid MD5 and SHA-1 for new security-sensitive designs.
3. Do not use plain SHA-256 alone for password storage.
4. Use a reputable password-hashing method with appropriate parameters.
5. Generate a unique random salt for each password record.
6. If using a pepper, store it separately and protect it as a secret.
7. Encourage long, unique passwords and password managers.
8. Enable MFA wherever possible.
9. Use rate limiting, monitoring, and appropriate login protections.
10. Protect password databases, backups, and application secrets.

---

# 8. 🧠 Quick Revision

- **Hash:** A fixed-size digest generated from input data.
- **SHA:** Secure Hash Algorithm; includes SHA-2 and SHA-3 families.
- **SHA-256:** A 256-bit cryptographic hash, useful for many purposes but not suitable alone for password storage.
- **Argon2id:** A modern password-hashing method with configurable memory and time costs.
- **bcrypt:** A password-hashing method with an adjustable work factor.
- **scrypt:** A memory-intensive password-hashing method.
- **PBKDF2:** A password-based key-derivation method that uses repeated operations.
- **Salt:** A random value stored with the password-hash record.
- **Pepper:** An additional secret stored separately from the hash database.
- **Online attack:** Password guesses are checked by the live login system.
- **Offline attack:** Guesses are checked locally against obtained password-hash data.
- **John the Ripper:** A tool that can test password candidates against supported hashes in authorized audits.

---

# 9. 📚 Learning Resources

These are the resources I used to study and understand hashing and password security.

- **NIST Secure Hash Standard (FIPS 180-4):** [Read the official standard](https://csrc.nist.gov/pubs/fips/180-4/upd1/final)
- **NIST SHA-3 Standard (FIPS 202):** [Read the official standard](https://csrc.nist.gov/pubs/fips/202/final)
- **OWASP Password Storage Cheat Sheet:** [Read the documentation](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- **John the Ripper Official Website:** [Explore the tool](https://www.openwall.com/john/)
- **NIST Digital Identity Guidelines:** [Read the guidelines](https://pages.nist.gov/800-63-4/sp800-63b.html)

---

## 🎯 My Learning Goal

I am learning cybersecurity step by step, focusing on understanding the concepts behind the tools rather than memorizing commands. These notes and visual posters are part of my effort to document my progress and make technical topics easier for other learners to understand.

**Disclaimer:** These notes are for educational purposes. Password auditing and security testing should only be performed on systems and data you own or have explicit permission to test.
