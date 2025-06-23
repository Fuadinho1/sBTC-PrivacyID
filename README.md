# sBTC-PrivacyID Identity Protocol (Stacks Smart Contract)

## Overview

The **sBTC-PrivacyID Identity Protocol** is a privacy-preserving identity verification system built on the Stacks blockchain. It leverages **zero-knowledge proofs (ZKP)** to enable identity verification without exposing sensitive information on-chain. The protocol supports decentralized identity management, proof registration, third-party verifiers, and multiple verification types (e.g., KYC, age, location).

---

## Key Features

* ✅ **Privacy-Preserving Verification:** Uses zero-knowledge proofs for secure, private validation.
* ✅ **Flexible Verification Types:** Supports KYC, age, location, and accredited status checks.
* ✅ **Identity Lifecycle Management:** Register, verify, and revoke identities.
* ✅ **Verifier Authorization:** Verifiers can be registered to handle specific types of identity checks.
* ✅ **Merkle Proof Support:** Uses Merkle roots to verify off-chain data efficiently.
* ✅ **Tamper-Resistant Proof Registry:** Proofs are linked to identities with explicit validity and expiry.

---

## Data Structures

### Identities

Each identity is mapped by a unique hash and contains:

* **Owner:** Principal who controls the identity.
* **Status:** Active, revoked, or expired.
* **Verification Types:** List of supported verification types.
* **Merkle Root:** For off-chain data verification.
* **Attestations:** Count of successful verifications.
* **Expiry Height:** Blockchain height at which the identity expires.

---

### Verifiers

Authorized third parties who can validate proofs.

* **Allowed Types:** Verification types they can process.
* **Activity Tracking:** Verifications performed and the last verification block.
* **Status:** Active or inactive.

---

### ProofRegistry

Stores hashes of submitted proofs with:

* **Verification Type**
* **Validity & Expiry**
* **Identity Association**

---

### VerificationRequests

Tracks submitted verification requests for potential extension or multi-step processes (not fully used in current implementation).

---

## Key Constants

* `STATUS-ACTIVE`: Identity is valid.
* `STATUS-REVOKED`: Identity has been revoked.
* `STATUS-EXPIRED`: Identity is expired.
* `TYPE-KYC`, `TYPE-AGE`, `TYPE-LOCATION`, `TYPE-ACCREDITED`: Supported verification types.
* Custom Errors: `ERR-UNAUTHORIZED`, `ERR-INVALID-PROOF`, `ERR-IDENTITY-EXISTS`, etc.

---

## Core Functions

### Identity Management

* `register-identity`: Register a new identity with its verification types and expiry.
* `add-proof`: Attach a zero-knowledge proof to an identity.
* `verify-identity`: Verifies a proof by an authorized verifier.
* `revoke-identity`: Allows identity owners or contract owners to revoke an identity.

### Verifier Management

* `register-verifier`: Allows the contract owner to register new verifiers with specific permissions.

### Read-Only Views

* `get-identity-info`: Retrieve identity details.
* `get-proof-info`: Retrieve proof details.
* `get-verifier-info`: Retrieve verifier details.
* `is-identity-valid`: Checks if an identity is active, not expired, and supports a verification type.

---

## Security & Access Control

* **Ownership Enforcement:** Only identity owners or contract owners can revoke identities.
* **Verifier Restrictions:** Only registered, active verifiers can process identity proofs.
* **Expiry Validation:** Both identities and proofs must be within their validity period.

---

## Suggested Enhancements

* Integration of signature-based verifications for external proof sources.
* Automated verification request processing using the `VerificationRequests` map.
* Time-based automatic expiration processing via a scheduled task or miner incentives.

---

## Deployment Notes

* Contract owner is set as the deployer (`tx-sender` at deploy time).
* Ensure verifier principals and allowed verification types are managed carefully to prevent unauthorized access.

---

## Example Flow

1. **Identity Owner:**

   * Calls `register-identity` to create an identity.

2. **Identity Owner:**

   * Calls `add-proof` to register a zero-knowledge proof for the identity.

3. **Verifier:**

   * Calls `verify-identity` to validate the proof.

4. **Identity Owner or Contract Owner:**

   * Calls `revoke-identity` if the identity needs to be revoked.

---

## Error Codes

| Code | Description               |
| ---- | ------------------------- |
| u1   | Unauthorized access       |
| u2   | Invalid proof             |
| u3   | Identity already exists   |
| u4   | Identity not found        |
| u5   | Expired identity or proof |
| u6   | Verifier not permitted    |
| u7   | Identity has been revoked |
| u8   | Proof verification failed |

---
