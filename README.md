# ProofPass

### Blockchain-Verified Digital Credentials

ProofPass is a blockchain-backed credential verification platform that makes digital certificates easier to issue, share, and verify.

Instead of relying solely on a traditional database, ProofPass creates a cryptographic proof of each credential and records that proof on the **Ethereum Sepolia testnet**. Anyone with the credential's verification link or hash can independently verify whether it is authentic or has been revoked.

---

## 🚀 Overview

Digital certificates are often difficult to verify and can be vulnerable to forgery or manipulation.

ProofPass addresses this by connecting a traditional credential system with blockchain verification.

The platform allows an issuer to:

1. Create a digital credential.
2. Generate a unique cryptographic hash for the credential.
3. Record the hash on the Ethereum blockchain.
4. Share the credential through a dedicated verification page and QR code.
5. Allow anyone to verify the credential.
6. Revoke credentials when they are no longer valid.

### Core flow

```text
Issue
  ↓
Generate Credential Hash
  ↓
Record Proof on Ethereum
  ↓
Store Credential Metadata
  ↓
Share Credential + QR Code
  ↓
Verify On-Chain
```

---

## ✨ Features

### Credential Issuance

Issuers can create credentials by providing:

* Holder name
* Credential name
* Issuer name

ProofPass generates a cryptographic hash from the credential data and records it on Ethereum.

### 🔐 Blockchain Verification

Credential authenticity is verified against the proof stored on the Ethereum Sepolia blockchain.

This provides an independent verification layer rather than relying exclusively on the application's database.

### 📱 QR Code Verification

Every issued credential has a QR code that links directly to its verification page.

A verifier can scan the code and immediately check the credential's status.

### 🚫 Credential Revocation

Issuers can revoke credentials when necessary.

Once revoked, the credential is treated as invalid during verification.

### 🔎 Public Verification

Verification does not require an account.

A verifier can use a credential hash or QR code to check:

* Credential holder
* Credential type
* Issuer
* Verification status
* Blockchain issuer
* Issuance information
* Blockchain transaction

### 🌐 Ethereum Sepolia

ProofPass currently uses the **Ethereum Sepolia testnet**, allowing the application to demonstrate real blockchain transactions without using mainnet funds.

---

## 🏗️ Architecture

ProofPass uses a three-layer architecture:

```text
                 ┌──────────────────────┐
                 │      React Frontend  │
                 │      Vite + CSS      │
                 └──────────┬───────────┘
                            │
                            │ REST API
                            ▼
                 ┌──────────────────────┐
                 │    Express Backend   │
                 │                      │
                 │ Credential API       │
                 │ Hash Generation      │
                 │ Database Operations  │
                 │ Blockchain Interface │
                 └───────┬───────┬──────┘
                         │       │
                    PostgreSQL   │
                         │       │
                         ▼       ▼
                  ┌──────────┐  ┌─────────────────┐
                  │ Database │  │ Ethereum Sepolia│
                  └──────────┘  │   ProofPass     │
                                │    Contract     │
                                └─────────────────┘
```

### Frontend

Built with:

* React
* Vite
* React Router
* CSS
* `react-qr-code`

The frontend provides the credential issuance, credential display, QR verification, and verification interfaces.

### Backend

Built with:

* Node.js
* Express
* PostgreSQL
* Ethers.js

The backend handles:

* Credential creation
* Credential hashing
* Database persistence
* Blockchain transactions
* Credential verification
* Credential revocation

### Blockchain

ProofPass uses a Solidity smart contract deployed on Ethereum Sepolia.

The contract stores credential proofs and provides functions for:

* Issuing credentials
* Verifying credentials
* Revoking credentials

---

## 🔗 How Verification Works

When an issuer creates a credential, ProofPass combines the credential information into a deterministic data structure and generates a cryptographic hash.

For example:

```text
Holder:
Ahmed Ibrahim

Credential:
Blockchain Developer Certificate

Issuer:
ProofPass
```

The resulting data is hashed using Ethereum's `keccak256` hashing mechanism.

The resulting hash becomes the credential's blockchain proof:

```text
0xa0651c33...
```

That proof is recorded by the smart contract.

When someone verifies the credential, ProofPass:

1. Retrieves the credential from the database.
2. Queries the blockchain using the credential hash.
3. Checks whether the blockchain proof is valid.
4. Checks whether the credential has been revoked.
5. Returns the verification result.

This creates a bridge between application-level credential data and blockchain-backed verification.

---

## 🛠️ Tech Stack

| Layer                  | Technology        |
| ---------------------- | ----------------- |
| Frontend               | React             |
| Build Tool             | Vite              |
| Styling                | CSS               |
| Backend                | Node.js + Express |
| Database               | PostgreSQL        |
| Blockchain Interaction | Ethers.js         |
| Smart Contract         | Solidity          |
| Blockchain             | Ethereum Sepolia  |
| QR Codes               | react-qr-code     |
| Backend Hosting        | Render            |
| Frontend Hosting       | Vercel            |

---

## 📁 Project Structure

```text
trust-pass/
│
├── README.md
│
└── proofpass/
    │
    ├── backend/
    │   ├── src/
    │   │   ├── abi/
    │   │   │   └── ProofPass.json
    │   │   │
    │   │   ├── db/
    │   │   │   └── db.js
    │   │   │
    │   │   ├── blockchain.js
    │   │   └── server.js
    │   │
    │   └── package.json
    │
    ├── blockchain/
    │   ├── contracts/
    │   │   └── ProofPass.sol
    │   │
    │   ├── scripts/
    │   │   └── deploy.ts
    │   │
    │   ├── test/
    │   │   └── ProofPass.ts
    │   │
    │   └── hardhat.config.ts
    │
    └── frontend/
        ├── public/
        ├── src/
        │   ├── App.jsx
        │   ├── App.css
        │   ├── index.css
        │   └── main.jsx
        │
        ├── index.html
        └── package.json
```

---

## ⚙️ Local Development

### Prerequisites

Make sure you have installed:

* Node.js
* npm
* PostgreSQL
* Git

You will also need an Ethereum Sepolia RPC endpoint and a funded Sepolia wallet for blockchain transactions.

### 1. Clone the repository

```bash
git clone https://github.com/Nobody-983/trust-pass.git
cd trust-pass
```

### 2. Install backend dependencies

```bash
cd proofpass/backend
npm install
```

Create a `.env` file:

```env
PORT=4000

DATABASE_URL=your_postgresql_connection_string

SEPOLIA_RPC_URL=your_sepolia_rpc_url

PRIVATE_KEY=your_wallet_private_key

CONTRACT_ADDRESS=your_deployed_contract_address
```

> Never commit your `.env` file or expose your private key.

Start the backend:

```bash
npm run dev
```

The API will run on:

```text
http://localhost:4000
```

### 3. Install frontend dependencies

Open another terminal:

```bash
cd proofpass/frontend
npm install
```

Start the frontend:

```bash
npm run dev
```

Vite will provide the local development URL.

---

## 🔌 API Endpoints

### Issue Credential

```http
POST /api/credentials
```

Request:

```json
{
  "holderName": "Ahmed Ibrahim",
  "credentialName": "Blockchain Developer Certificate",
  "issuerName": "ProofPass"
}
```

### Verify Credential

```http
GET /api/credentials/:hash
```

Returns the stored credential together with its blockchain verification result.

### Revoke Credential

```http
POST /api/credentials/:hash/revoke
```

Revokes the credential on-chain and updates its database status.

---

## 🔒 Security Considerations

ProofPass separates sensitive blockchain credentials from the frontend.

The blockchain signing wallet's private key is kept exclusively on the backend through environment variables.

The frontend never receives the private key.

The application also uses parameterized PostgreSQL queries to reduce the risk of SQL injection.

> ProofPass is currently a hackathon MVP and should undergo additional security auditing before being used for production credentials.

---

## 🧪 Current Network

ProofPass currently operates on:

**Ethereum Sepolia Testnet**

This project is intended for demonstration and testing purposes. Credentials created during the demonstration are not intended to represent official real-world certificates.

---

## 🎯 Use Cases

ProofPass can be adapted for:

* Educational certificates
* Course completion certificates
* Professional certifications
* Workshop participation
* Training programs
* Event credentials
* Developer certifications
* Digital achievement records

---

## 🔮 Future Improvements

Potential future development includes:

* Issuer authentication and organization accounts
* Wallet-based issuer identity
* Decentralized credential storage
* Credential expiration dates
* Batch credential issuance
* More advanced issuer permissions
* Email-based credential delivery
* NFT-based credential representations
* Multi-chain support
* Production-grade smart contract auditing
* Decentralized storage such as IPFS

---

## 📌 Project Status

**ProofPass is currently an MVP built for hackathon demonstration.**

The core issuance, blockchain anchoring, verification, QR sharing, and revocation flows are implemented.

### Core flow

**Issue → Anchor → Share → Verify**

---

## 👨‍💻 Author

**Ahmed Ibrahim**

Computer Science Student
University of Ilorin

GitHub: `Nobody-983`

---

## 📄 License

This project is licensed under the MIT License.
