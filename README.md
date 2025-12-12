# Multi-Sig Governance & Resilience Plan

## 1. System Overview

**Wallet Name:** [e.g. Vault Alpha]
**Protocol:** Bitcoin (Sparrow Wallet / Bitcoin Core)
**Configuration:** **2-of-3** Multi-Signature
**Creation Date:** [YYYY-MM-DD]
**Network:** Bitcoin Mainnet

### Primary Threat Model

This architecture is specifically hardened against two catastrophic scenarios:

1. **Coercion at Home (The "$5 Wrench Attack"):**
   * **Threat:** An attacker forces me to sign a transaction while I am at `ZA-HOME`.
   * **Defense:** Spending requires **2 signatures**. Only **1 key** is accessible at home (Device A). The second key (`ZA-OFFSITE`) is geographically separated.
   * **Result:** Immediate spending is impossible. The attacker is forced to transport me to a second location, drastically increasing their risk and exposure.

2. **National-Level Confiscation (Total Zone A Compromise):**
   * **Threat:** A hostile state actor seizes all physical assets within my primary jurisdiction (Zone A).
   * **Defense:** The system spans two adversarial jurisdictions.
   * **Recovery Path:** If all physical hardware in Zone A is seized or destroyed, I flee to **Zone B**. I retrieve **Key C** and the **Encrypted Backup of Key A**. I use my **Memorized Password** to decrypt Key A and reconstruct the wallet.

### Security Philosophy & Mitigations

* **Simplicity & Uniformity (User Error Reduction):**
  * **Uniform Hardware:** We use **Coldcard Q1** for all three signers.
  * **Unified Credentials:** We use the **same BIP39 Passphrase** and the **same Encryption Password** for all devices and backups.
  * **Rationale:** Complexity is a security risk. By standardizing the secrets, we drastically reduce the chance of lockout.

* **Cryptography over Obscurity:**
  * **Data Security:** All sensitive data (descriptors, XPUBs) is strictly encrypted (AES-256).
  * **Clear Labeling:** We label all physical items clearly. Information is encrypted, not hidden.
  * **Access Security:** We rely on **Operational Compartmentalization** within Zone A; no single location holds enough keys to spend.
  * **The Anchor Exception:** **Zone B** is the only location holding the **Master Index** (map of all locations). This is necessary for recovery if I die or lose my memory.

* **Zone A Total Loss Resilience (Encrypted Mirror):**
  * **Strategy:** We store an **Encrypted MicroSD Backup** of Key A in Zone B (`ZB-OFFSITE`).
  * **Memory Role:** I memorize the **Encryption Password** for this file.
  * **Risk Mitigation:** If I forget the password, **no assets are lost** provided Zone A is still intact. This memory component is strictly a redundant measure for the "Total Zone A Loss" scenario.

* **Theft Tolerance & Attack Vectors:**
  * **Device Theft:** To steal funds, an attacker must physically compromise **2 separate locations** (e.g., `ZA-HOME` + `ZA-OFFSITE`).
  * **Coercion (The "$5 Wrench"):** Since **NO seeds are memorized**, coercion at a single location is futile. An attacker cannot extract a key from my brain. They are forced to transport me to a second physical location (`ZA-OFFSITE`), drastically increasing their exposure.
  * **Hot Zone Elimination:** Unlike previous iterations, I do not memorize Key A. Therefore, being present at `ZA-OFFSITE` does **not** create a risk of having 2 keys in one room.

* **The "Brain is Not a Vault" Rule:**
  * We use the brain for redundancy (e.g., memorizing the backup password), but **never** as the sole storage for primary access.
  * **Hard Copies:** Every secret (seed, passphrase, password) exists physically in Zone A.

* **Coercion Resistance (The "No-Go" Rule):**
  * **Rule:** I do **NOT** memorize the seed for **any** key.
  * **Nuance:** I memorize the **Encryption Password** for the backup, but this password is useless without the specific **Encrypted MicroSD Backup** (located in Zone B). It has **no interaction** with the physical hardware devices.
  * **Result:** Spending *always* requires physical travel to `ZA-OFFSITE`.

* **Privacy as Security:**
  * **Encrypted Descriptors:** No wallet descriptors (XPUBs) or configuration QR codes are left in plain sight.

## 2. Key Inventory & Zonal Topology

*To protect physical security, locations are coded. Zone A is the operational hub; Zone B is the survival anchor.*

| Zone | Key Alias | Hardware Model | Location Code | Storage Role |
| :--- | :--- | :--- | :--- | :--- |
| **A** | **Key A** | Coldcard Q1 | `ZA-HOME` | **Signer 1:** Device + Steel Backup. |
| **A** | **Key B** | Coldcard Q1 | `ZA-OFFSITE` | **Signer 2:** Device stored here. Seed is **NOT** memorized. |
| **B** | **Key C** | Coldcard Q1 | `ZB-HOME` | **Signer 3:** Device + Steel Backup. |
| **B** | **Key A** | Encrypted MicroSD | `ZB-OFFSITE` | **Mirror:** Encrypted Backup (Key A) + **2x M-DISC** (Master Index). |

### Watch-Only Interface

* **Primary Software:** Sparrow Wallet (Recommended for Coldcard)
* **Mobile Watch-Only:** BlueWallet or Nunchuk (Watch-only XPUB import)

## 3. Operational Policy

* **Zone A Exclusivity:** Routine spending must occur entirely within Zone A.
* **Travel Requirement:** Signing requires physical travel between `ZA-HOME` and `ZA-OFFSITE`.
* **Procedure:** See `procedures/signing.md` for the step-by-step guide.

## 4. Maintenance Policy

* **Schedule:** Quarterly (1st of Jan, Apr, Jul, Oct).
* **Scope:** Hardware health (power cycle), software updates, and memory drills (encryption password).
* **Procedure:** See `procedures/maintenance.md` for the full checklist.

## 5. Recovery Policy

**IF I AM INCAPACITATED OR DECEASED:**

This repository explains the *structure* of the funds, but it does not hold the funds.

* **Master Index:** The physical map of all key locations is stored on **M-DISC** at `ZB-OFFSITE`.
* **Inheritance:** Heirs must retrieve the Master Index to locate keys. See `recovery/inheritance.md`.
* **Zone A Destruction:** Execute **Encrypted Mirror Recovery** using `ZB-HOME` (Key C) + `ZB-OFFSITE` (Backup) + Password.
* **Procedure:** See `recovery/execution.md` for step-by-step guides.
