# 🎓 E-Certificate Validation & Verification using Blockchain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Solidity](https://img.shields.io/badge/Solidity-^0.8.20-363636.svg)](https://soliditylang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-14.x-black.svg)](https://nextjs.org/)
[![Ethers.js](https://img.shields.io/badge/Ethers.js-v6.x-blueviolet.svg)](https://docs.ethers.org/v6/)

**CertifyChain** is a decentralized, tamper-proof e-certificate issuance, management, and verification platform built on the Ethereum blockchain. It eliminates academic and professional certificate fraud by anchoring certificate metadata and cryptographic hashes to smart contracts, enabling instant, single-click verification for third parties and employers worldwide.

---

## 🌟 Key Features

- **🛡️ Tamper-Proof Storage**: Cryptographic certificate hashes stored immutably on the blockchain network.
- **⚡ Instant Verification**: Single-click verification via unique Certificate Hash or QR Code.
- **👑 Role-Based Access Control**: Secure separation between University Admins, Students, and Public Verifiers.
- **📄 Dynamic PDF & QR Code**: Automated certificate generation embedded with verification URLs.
- **⛽ Gas-Optimized Smart Contract**: Efficient state management and event indexing using Solidity 0.8.x.

---

## 🏗️ System Architecture

```
                                +---------------------------+
                                |    Third-Party / Public   |
                                +-------------+-------------+
                                              |
                                     (1) Verifies Certificate ID/QR
                                              v
+------------------+   (2) Issues   +-------------------+   (3) Stores Hash   +---------------------+
| University Admin | -------------> |  Next.js Frontend | ------------------> |  Solidity Contract  |
+------------------+  Certificate   +---------+---------+    & Log Events     | (CertificateRegistry)|
                                              |                               +----------+----------+
                                     (4) Query Status                                    |
                                              v                                          v
                                    +-------------------+                      +--------------------+
                                    |  Student Portal   |                      |  Ethereum / Polygon|
                                    +-------------------+                      |   Testnet Ledger   |
                                                                               +--------------------+
```

---

## 🔄 Workflow Diagram

```
[ University Admin ]
         │
         ├──> 1. Inputs Student Details & Certificate ID
         ├──> 2. Generates SHA-256 Hash of Certificate Data
         └──> 3. Executes `issueCertificate()` on Smart Contract
                     │
                     ▼
             [ Ethereum Network ]
                     │
                     ├─> Emits `CertificateIssued` Event
                     └─> Stores Struct Mapping in Immutable Ledger
                     │
                     ▼
[ Third-Party / Employer Verifier ]
         │
         ├──> 1. Inputs Certificate ID / Scans QR Code
         ├──> 2. Calls `verifyCertificate()` on Smart Contract
         └──> 3. View Instant Validation Status (Valid / Revoked / Invalid)
```

---

## 🛠️ Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Blockchain** | Solidity, Hardhat, Ethers.js, Sepolia Testnet |
| **Frontend** | PHP |
| **Storage / DB** | SQL |

---

