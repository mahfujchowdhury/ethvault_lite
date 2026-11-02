# ETHVault Lite

ETHVault Lite is an offline-only Ethereum signing tool built in Python. It helps you create and restore wallets, review unsigned Ethereum transactions, sign them without exposing the private key to the network, and pair with wallet apps using QR-based EIP-4527 flows.

This project is designed for air-gapped or offline use: it blocks all socket/network access, requires no RPC, and only works with QR or file transfer workflows.

## Why this project exists

ETHVault Lite is meant to act like a minimal, transparent, offline hardware-wallet-style signer for Ethereum transactions. It focuses on:

- generating or restoring wallets
- encrypting wallets to keystore files
- reviewing and approving transaction details before signing
- producing QR payloads for wallet apps to scan
- pairing with MetaMask, Rabby, OKX, Bitget, and Keystone-compatible flows
- staying completely offline while the transaction is constructed elsewhere

## Core features

- Create a new wallet or restore from a 12/15/18/21/24-word seed phrase
- Save encrypted keystore JSON files with a password
- Review Ethereum transactions before signing
- Sign EIP-1559 and legacy EIP-155 transactions
- Show signed transaction QR payloads for the requesting wallet app
- Pair a seed-derived account with wallet apps using QR-based EIP-4527 / UR payloads
- Explicitly hard-block network access in the app runtime
- Self-test logic for wallet create/restore, pairing, and signing flows

## Important safety note

This is an educational and experimental tool. It is not a production-ready audited wallet implementation.

Use it only with:

- testnets
- small amounts
- offline devices
- trustworthy seed phrases and keystore backups

Never expose your seed phrase or keystore on a connected machine. Never use it on a shared or compromised environment.

## Project structure

- `ethvault_lite.py` — Tkinter GUI app; the main user-facing offline wallet interface
- `vault_core.py` — core wallet logic, transaction review, signing, pairing, and validation
- `evur.py` — Ethereum UR / EIP-4527 helpers for sign requests, signature payloads, and pairing QR generation
- `ur.py` — generic UR / bytewords codec shared by the project

## Requirements

Install Python dependencies:

```bash
pip install eth_account cbor2 rlp
```

Optional for QR display and scanning:

```bash
pip install qrcode pillow
pip install pyzbar opencv-python
```

## Quick start

Run the graphical app:

```bash
python ethvault_lite.py
```

Run the logic-only self-test:

```bash
python vault_core.py
```

## How it works

ETHVault Lite follows a strong offline signer pattern:

1. The wallet is created or restored on an air-gapped/offline machine.
2. The keystore is encrypted and saved locally.
3. A wallet app (such as MetaMask or Rabby) builds an unsigned transaction.
4. The app displays a QR request (`ur:eth-sign-request`).
5. ETHVault Lite reviews the transaction and asks for confirmation.
6. The user signs the transaction using the local keystore.
7. The signed payload is shown as a QR for the wallet app to broadcast.

## Supported flows

- Wallet creation
- Wallet restoration from seed phrase
- Signing Ethereum transactions
- Pairing via QR with wallet apps
- EIP-1559 and legacy EIP-155 transaction support

## Typical usage

### 1. Create wallet

- Open the app
- Go to the Create Wallet tab
- Enter a strong password
- Create a random wallet or create one from a 24-word seed phrase
- Save the keystore JSON to a local file

### 2. Sign a transaction

- Open the Sign Tx tab
- Scan or paste a `ur:eth-sign-request`
- Select the keystore JSON file
- Enter the keystore password
- Review the transaction details
- Confirm and sign
- Display the signature QR to the requesting wallet app

### 3. Pair with a wallet app

- Open the Pair tab
- Enter the seed phrase
- Choose a wallet profile
- Show the pairing QR in the target wallet app

## File overview

### `ethvault_lite.py`

The main graphical interface. It hard-disables networking and provides the three tabs:

- Create Wallet
- Sign Tx
- Pair (EIP-4527)

### `vault_core.py`

Contains the true logic layer for:

- keystore creation/restoration
- network labeling and transaction review
- signing and verification
- pairing payload generation
- self-tests

### `evur.py`

Handles Ethereum-specific UR payload encoding/decoding, signature flows, and account derivation for EIP-4527 wallet pairing.

### `ur.py`

Generic Uniform Resource codec using bytewords and CBOR support for UR payloads used by QR hardware wallet workflows.

## Notes

- The app intentionally does not connect to any RPC endpoint or broadcast transactions.
- This project is intended as a local, air-gapped signing helper.
- The transaction is built and broadcast by another wallet application, not by ETHVault Lite.

## License

No explicit license file is present in the repository. Please review repository ownership and usage terms before redistributing or using the code in production.

## Disclaimer

ETHVault Lite is provided as-is, without warranty of any kind. The project is not audited and should be treated as experimental software.

Use at your own risk.
