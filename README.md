# ⊕ Offline Seed XOR Calculator (BIP-39)

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Production Ready](https://img.shields.io/badge/Status-Production_Ready-success)
![Security: Air-gapped](https://img.shields.io/badge/Security-Air--Gapped_Only-red)

[🇬🇧 English](#english) | [🇹🇭 ภาษาไทย](#ภาษาไทย)

🌐 **Live Demo (Educational Purpose Only):** [https://chontit.github.io/seed-xor-calculator/](https://chontit.github.io/seed-xor-calculator/) *(Please replace with your actual GitHub Pages link)*

---

<h2 id="english">🇬🇧 English</h2>

A highly secure, 100% offline, single-file HTML tool for calculating Bitwise XOR for BIP-39 Seed Phrases. Designed with Paranoia-level OPSEC in mind. Ideal for running on air-gapped systems like Tails OS.

### ❓ What is Seed XOR?
Seed XOR is an advanced cryptographic method used to split a single Master Seed (BIP-39) into multiple "Shares" using Bitwise Exclusive OR (XOR) operations. 
Unlike simply cutting a piece of paper in half (which dangerously reduces entropy), Seed XOR preserves absolute mathematically perfect secrecy (Zero Knowledge). 
* **All-or-Nothing (M-of-M):** If you split a seed into 3 shares, you absolutely need all 3 shares to recover the Master Seed.
* **Plausible Deniability:** The magic of Seed XOR is that **each individual share is a 100% valid BIP-39 seed phrase on its own.** You can fund these shares with small amounts of Bitcoin to act as "Decoy Wallets" against $5 Wrench Attacks.

### ✨ Features
- **100% Offline & Zero Dependencies:** No external libraries, no CDNs. The entire logic, including the 2048 BIP-39 English wordlist, is embedded in one HTML file.
- **Built-in SHA-256 Engine:** Accurately recalculates the 8-bit checksum for the final word of the Master Seed using native JavaScript.
- **Flexible Shares (M-of-M):** Supports combining anywhere from 2 to 10 seed shares.
- **Transparent Calculation (Don't Trust, Verify):** Displays step-by-step bit-level XOR operations (11-bit per word) so you can mathematically verify the output manually.
- **Educational Demo Mode:** Automatically detects if the file is being run on a networked server and prevents execution by default to protect users, with an opt-in mode for educational purposes.

### 📥 How to Download and Use
*For the highest level of security, **DO NOT** run this tool on a computer connected to the internet with real funds.*
1. **Download the file:** Right-click on the `index.html` file in this repository and select "Save link as...".
2. Transfer the file to a clean, offline USB drive.
3. Boot your offline machine (e.g., Tails OS) with internet physically disabled (unplug LAN, turn off Wi-Fi).
4. Open the `index.html` file using a Tor Browser (Offline Mode).
5. Input your shares, calculate the Master Seed, write it down on paper or steel.
6. **Shut down the computer immediately.** This completely wipes the RAM, leaving zero digital footprint.

### 🔐 File Verification (Checksums)
To ensure the file has not been tampered with or intercepted, verify the checksums before use:
**Filename:** `index.html`
* **SHA256:** `00fa0af4997d0ace1cd4ec57cf7be906e8d80de8eaca335716576d5a7397bcae`
* **MD5:** `b778e002d05353476d5ecfc503172db9`

#### How to Verify on Different Operating Systems:
Open your terminal or command prompt in the folder where the file is downloaded and run the following commands:

**Windows (PowerShell):**
```powershell
Get-FileHash .\index.html -Algorithm SHA256
Get-FileHash .\index.html -Algorithm MD5
