# ⊕ Offline Seed XOR Calculator (BIP-39)

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Production Ready](https://img.shields.io/badge/Status-Production_Ready-success)
![Security: Air-Gapped Only](https://img.shields.io/badge/Security-Air--Gapped_Only-red)
![Offline: 100%](https://img.shields.io/badge/Offline-100%25_No_CDN-blue)

[🇬🇧 English](#-english) | [🇹🇭 ภาษาไทย](#-ภาษาไทย)

> 🌐 **Live Demo (Educational Purpose Only):**
> [https://chontit.github.io/seed-xor-calculator/](https://chontit.github.io/seed-xor-calculator/)
> ⚠️ *Never enter your real seed phrase on a networked environment. Demo mode only.*

---

## 🇬🇧 English

A highly secure, **100% offline**, single-file HTML tool for reconstructing a BIP-39 Master Seed from multiple shares using **Bitwise XOR** operations. Designed with paranoia-level OPSEC in mind. Built for air-gapped systems like **Tails OS**.

Co-developed with Claude AI · Audited by Gemini AI · Built by [@chontit](https://github.com/chontit)

---

### ❓ What is Seed XOR?

Seed XOR is a cryptographic method that uses **Bitwise Exclusive OR (XOR)** to split a single Master Seed into multiple independent "Shares" — and reconstruct it only when all shares are combined.

Unlike physically cutting a paper wallet in half (which dangerously reduces entropy and leaks partial information), Seed XOR provides **mathematically perfect secrecy**:

| Property | Seed XOR | Cut Paper in Half |
|---|---|---|
| Entropy preserved | ✅ 256-bit full | ❌ ~128-bit each half |
| All-or-Nothing | ✅ Yes | ❌ No |
| Each part looks valid | ✅ Yes (decoy wallet) | ❌ Obviously incomplete |
| Verifiable math | ✅ Open, auditable | ❌ None |

**Key properties:**

- **All-or-Nothing (M-of-M):** Split into 3 shares → need all 3 to recover. Losing 1 share means the funds are permanently inaccessible. An attacker holding 2 of 3 shares gains **zero** information about the third.
- **Plausible Deniability ($5 Wrench Attack protection):** Each individual share is a **100% valid BIP-39 seed phrase** on its own. You can fund shares with small amounts of Bitcoin as "Decoy Wallets." If coerced, hand over a share — the attacker cannot distinguish it from a real wallet.

---

### ⚔️ Seed Backup Methods Compared: Multisig vs. Shamir Backup vs. Seed XOR

There are three main cryptographic methods for securing a Bitcoin seed with redundancy and distribution. Each solves the same core problem — "no single point of failure" — but with fundamentally different approaches, trade-offs, and complexity levels.

#### 🔑 Quick Summary

| Criteria | Multisig | Shamir Backup (SLIP-39) | Seed XOR (BIP-39) |
|---|---|---|---|
| Standard | Bitcoin Script / BIP-11 | SLIP-39 (Satoshi Labs) | BIP-39 compatible |
| Scheme type | M-of-N (e.g. 2-of-3) | M-of-N (e.g. 2-of-3) | M-of-M (all required) |
| Threshold recovery | ✅ Yes (M < N) | ✅ Yes (M < N) | ❌ No (all shares required) |
| Hardware wallet support | ✅ Wide (Trezor, Coldcard, Ledger) | ⚠️ Limited (Trezor Model T/Safe) | ✅ Any BIP-39 wallet |
| Tool required to reconstruct | ✅ Wallet software | ✅ SLIP-39 tool | ✅ XOR calculator (this tool) |
| Plausible deniability | ❌ None | ❌ Shares look like shares | ✅ Each share is a valid seed |
| Mathematical proof of security | ✅ Shamir's Secret Sharing | ✅ Shamir's Secret Sharing | ✅ Information-theoretic |
| Complexity | 🔴 High | 🟡 Medium | 🟢 Low |
| Single point of failure | ✅ Eliminated | ✅ Eliminated | ⚠️ Eliminated only if M=N |
| On-chain footprint change | 🔴 Yes (new address format) | ❌ No | ❌ No |
| Works with existing seed | ❌ Requires new wallet setup | ❌ Requires new seed generation | ✅ Works with existing seed |

---

#### 1️⃣ Multisig (Multi-Signature)

**How it works:** Instead of a single private key controlling a wallet, Multisig requires **M signatures from N different keys** to authorize a transaction. Example: 2-of-3 means any 2 of 3 keys can sign. Each key is a completely separate device or seed.

**Strengths:**
- **True M-of-N threshold:** Lose 1 key in a 2-of-3 setup and funds are still recoverable with the remaining 2
- **Mathematically proven security** at the transaction level — no single device compromise loses funds
- **Battle-tested in production** — used by exchanges, institutions, and advanced Bitcoiners for years
- **Requires attacker to compromise multiple independent devices** simultaneously

**Weaknesses:**
- **High complexity:** Requires careful coordination of multiple hardware wallets, xpubs, and wallet descriptors. A mistake in backup can permanently lock funds
- **On-chain footprint change:** Multisig uses P2SH or P2WSH addresses, which look different from standard P2WPKH — identifiable on-chain
- **Cannot use with an existing single-sig seed** — requires complete wallet migration
- **Inheritance/recovery complexity:** Heirs must understand how to combine keys and use the correct wallet software
- **No plausible deniability:** Owning a multisig wallet is obvious on-chain
- **Vendor lock-in risk:** If a specific hardware wallet model becomes discontinued or the software changes, recovery may be complicated

**Best for:** High-value cold storage, institutional Bitcoin custody, users comfortable with technical complexity.

---

#### 2️⃣ Shamir's Secret Sharing / Shamir Backup (SLIP-39)

**How it works:** Uses **Shamir's Secret Sharing (SSS)** algorithm to mathematically split a secret into N shares, where any M shares can reconstruct the original. Unlike XOR, SSS works with polynomial interpolation over a finite field — meaning any M-of-N subset (not all) is sufficient.

**Strengths:**
- **True M-of-N threshold with M < N:** Example: 2-of-5 means any 2 of 5 shares recover the seed — provides genuine redundancy. Losing some shares does not lose access
- **Mathematically proven:** Based on Shamir's Secret Sharing, a well-established academic cryptographic primitive
- **Hardware wallet integration:** Trezor Model T and Trezor Safe natively support SLIP-39 generation and recovery
- **No single point of failure** with genuine redundancy (M < N)

**Weaknesses:**
- **Limited hardware support:** Primarily Trezor. Coldcard, Ledger, and most other wallets do not support SLIP-39 natively as of 2025
- **Cannot use with an existing BIP-39 seed:** SLIP-39 generates a different seed — you must start fresh and migrate funds
- **Shares are obviously shares:** A SLIP-39 share looks nothing like a normal BIP-39 seed phrase — no plausible deniability
- **Reconstruction requires SLIP-39 compatible software:** Not as universally available as BIP-39 tools
- **Trust in implementation:** The split must be done on a trusted device; if the Trezor is compromised during share generation, all shares are compromised

**Best for:** Users with Trezor devices who want genuine M-of-N threshold recovery, willing to start with a fresh seed.

---

#### 3️⃣ Seed XOR (BIP-39 XOR)

**How it works:** Uses **Bitwise XOR** on the raw entropy bits of BIP-39 seed phrases. Split: generate random Share A and Share B such that `A XOR B = Master Seed`. Reconstruct: `Share A XOR Share B = Master Seed`. Each share is a independently valid BIP-39 seed. The XOR operation preserves all entropy bits — the security is information-theoretic (proven impossible to reconstruct without all shares).

**Strengths:**
- **Works with any existing BIP-39 seed:** No need to migrate. Split your current seed today without creating a new wallet
- **Any BIP-39 wallet can verify shares:** Load any share into Trezor, Coldcard, Ledger, or any hardware wallet to verify it opens a (decoy) wallet
- **Plausible deniability:** Each share is indistinguishable from a real seed. Fund shares with small amounts — hand over under coercion without revealing the master
- **Zero new on-chain footprint:** The Master Seed produces the same addresses — no wallet migration needed
- **Simple and auditable math:** XOR is one of the simplest binary operations — verifiable by hand or this open-source tool
- **No vendor dependency:** No proprietary format, works with open standards forever
- **Information-theoretic security:** Proven that knowing M-1 shares reveals mathematically zero information about the master seed

**Weaknesses:**
- **M-of-M only (no threshold):** All-or-Nothing. If you create 3 shares and lose 1, funds are permanently inaccessible. There is no "I only need 2-of-3" recovery
- **Manual process requires a trusted tool:** Must use a correct XOR calculator. A buggy tool (e.g., one that ignores checksum recomputation) produces an incorrect Master Seed that appears valid but opens the wrong wallet
- **No native hardware wallet support for splitting:** The split must be done externally (this tool). Hardware wallets only handle the individual BIP-39 shares as normal seeds
- **Coldcard support:** Coldcard firmware does support XOR natively — but other hardware wallets do not have a built-in XOR combine function

**Best for:** Bitcoin self-custody holders who want to split an existing seed across locations, value simplicity and auditability, need plausible deniability, and understand the all-or-nothing trade-off.

---

#### 🎯 Decision Guide: Which Method Should You Use?

```
Do you want to split an EXISTING seed without creating a new wallet?
├── Yes → Seed XOR is your only option
└── No (starting fresh)
    ├── Do you want M-of-N threshold (lose some shares, still recover)?
    │   ├── Yes → Shamir Backup (if you have Trezor) or Multisig
    │   └── No (all-or-nothing is acceptable) → Seed XOR
    ├── Do you need plausible deniability ($5 wrench attack protection)?
    │   ├── Yes → Seed XOR
    │   └── No → Any method
    └── Do you need institutional-grade security with multiple independent devices?
        ├── Yes → Multisig
        └── No → Seed XOR or Shamir
```

> **The honest answer:** For most individual Bitcoin self-custody users, **Seed XOR is the best balance of security, simplicity, and compatibility** — with the critical caveat that you must understand and accept the all-or-nothing trade-off.

---

### ✨ Features

| Feature | Detail |
|---|---|
| 🔌 100% Offline | No CDN, no external requests. Single `.html` file |
| 📖 BIP-39 Wordlist | All 2,048 words embedded directly in the file |
| 🔐 SHA-256 Engine | Pure JavaScript FIPS 180-4 implementation, no Web Crypto API |
| ✅ Checksum-aware XOR | Correctly recomputes checksum for the last word (7+4 or 3+8 bits) |
| 📏 Flexible Seed Length | Supports 12, 15, 18, 21, 24 words |
| 🔢 Flexible Shares | Supports 2 to 10 shares (M-of-M) |
| ⌨️ Autocomplete | Real-time BIP-39 word suggestions as you type |
| 📋 Paste Mode | Paste all words at once or type one by one |
| 🔍 Bit-level Verification | Step-by-step 11-bit XOR table with Binary, Decimal, and Word |
| 🛡️ Hard Stop on Bad Checksum | Cannot calculate if any share has an invalid checksum |
| 🌐 OPSEC Environment Check | Detects remote server and blocks by default (Demo Mode opt-in) |
| 🔥 Clear Session Button | One click to `location.reload()` — wipes all data from RAM |
| 📋 Clipboard Fallback | Works on `file://` and Tor Browser Safest mode (execCommand fallback) |

---

### 🧮 How BIP-39 XOR Works (Technical)

```
12 words = 132 bits = 128 bits entropy + 4 bits checksum
24 words = 264 bits = 256 bits entropy + 8 bits checksum

Word 1–11  : 11 bits each → pure entropy → XOR normally
Word 12    : 7 bits entropy (XOR) + 4 bits checksum (SHA-256 recomputed)

Word 1–23  : 11 bits each → pure entropy → XOR normally
Word 24    : 3 bits entropy (XOR) + 8 bits checksum (SHA-256 recomputed)
```

The checksum of the Master Seed is **always recomputed from the XOR'd entropy** — never derived from XOR-ing the checksums of individual shares.

SHA-256 self-test runs on every page load against known test vectors to verify the engine is working correctly before any calculation.

---

### 📥 How to Download and Use (Secure Mode)

> ⚠️ **NEVER use your real seed phrase on an internet-connected machine.**

1. **Download:** Click `Seed-XOR.html` in this repository → **Raw** → `Ctrl+S` to save
2. **Verify the file** (see Checksums section below) — confirm the file was not tampered with
3. Copy the file to a clean USB drive
4. Boot into **Tails OS** (or any offline OS)
5. **Physically disable all network** — unplug LAN cable, turn off Wi-Fi
6. Open `Seed-XOR.html` in the browser
7. Confirm SHA-256 self-test shows **green** before entering any data
8. Enter your shares, calculate the Master Seed
9. Write the result on paper or metal — **never type it anywhere digital**
10. Click **🔥 Clear Session** when done
11. **Shut down immediately** — RAM is wiped on power-off

---

### 🔐 File Verification (Checksums)

Verify before use — confirm the file has not been tampered with or injected with malware.

**Filename:** `Seed-XOR.html`

| Algorithm | Hash |
|---|---|
| **SHA-256** | `00fa0af4997d0ace1cd4ec57cf7be906e8d80de8eaca335716576d5a7397bcae` |
| **MD5** | `b778e002d05353476d5ecfc503172db9` |

#### Verify on your OS:

**Windows (PowerShell)**
```powershell
Get-FileHash .\Seed-XOR.html -Algorithm SHA256
Get-FileHash .\Seed-XOR.html -Algorithm MD5
```

**macOS (Terminal)**
```bash
shasum -a 256 Seed-XOR.html
md5 Seed-XOR.html
```

**Linux / Tails OS (Terminal)**
```bash
sha256sum Seed-XOR.html
md5sum Seed-XOR.html
```

---

### 🔒 Security Model

- **No network calls** — verified by inspecting the source: zero `fetch()`, `XMLHttpRequest`, or external `<script src>` tags
- **No localStorage / sessionStorage** — all state is in-memory only, wiped on page close
- **Input sanitization** — all words are validated against the BIP-39 wordlist (lowercase a–z only) before any DOM rendering → no XSS surface
- **Hard stop on invalid checksum** — calculation is blocked if any share fails BIP-39 checksum validation
- **Environment lock** — detects `https://` remote server and disables calculation by default; requires explicit Demo Mode activation
- **Defense in depth** — OPSEC lock enforced in 3 layers: `IS_UNSAFE_ENV` flag, `updateCalcBtn()`, and `calculate()` function

---

### 🤝 Credits & Acknowledgements

- **Author:** [Chollatis Bitcoiner](https://github.com/chontit) — RTAF Officer & Bitcoin educator, Korat Bitcoiner community
- **AI Co-pilot:** Claude (Anthropic) — architecture, code, security hardening
- **Security Audit:** Gemini (Google) — 4-round code review, edge case discovery
- **Bitcoin education platform:** [learning.chontit.win](https://learning.chontit.win)
- **BIP-39 Wordlist source:** [trezor/python-mnemonic](https://github.com/trezor/python-mnemonic)
- **Specification:** [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), [FIPS 180-4 SHA-256](https://csrc.nist.gov/publications/detail/fips/180/4/final)

---

## 🇹🇭 ภาษาไทย

เครื่องมือคำนวณ **Seed XOR** สำหรับ BIP-39 แบบออฟไลน์ 100% ในรูปแบบไฟล์เดียว (Single-file HTML) ออกแบบมาเพื่อความปลอดภัยสูงสุดระดับ Cypherpunk เหมาะสำหรับใช้งานบนระบบที่ปิดอินเทอร์เน็ต (Air-gapped) เช่น Tails OS

พัฒนาร่วมกับ Claude AI · ตรวจสอบโดย Gemini AI · สร้างโดย [@chontit](https://github.com/chontit)

---

### ❓ Seed XOR คืออะไร?

Seed XOR คือเทคนิคการเข้ารหัสที่ใช้ **คณิตศาสตร์ XOR ระดับบิต** เพื่อแยก Master Seed ออกเป็นหลายชิ้นส่วน (Shares) และกู้คืนได้เมื่อนำมารวมกันครบทุกชิ้น

**เปรียบเทียบกับการตัดกระดาษแบ่งครึ่ง:**

| คุณสมบัติ | Seed XOR | ตัดกระดาษครึ่ง |
|---|---|---|
| Entropy เต็ม 256-bit | ✅ ใช่ | ❌ แต่ละครึ่งมีแค่ ~128-bit |
| All-or-Nothing จริง | ✅ ใช่ | ❌ ไม่ใช่ |
| แต่ละส่วนดูเหมือน Seed จริง | ✅ ใช่ (ทำ Decoy ได้) | ❌ ชัดเจนว่าไม่สมบูรณ์ |
| ตรวจสอบคณิตศาสตร์ได้ | ✅ โปร่งใสทุก bit | ❌ ไม่มี |

**คุณสมบัติหลัก:**

- **All-or-Nothing (ต้องครบทุกส่วน):** แบ่งเป็น 3 ชิ้น ต้องใช้ครบทั้ง 3 ในการกู้คืน ถ้าหายไป 1 ชิ้น — เงินหายถาวร โจรขโมยได้ 2 ชิ้น ไม่ช่วยให้เดาชิ้นที่ 3 ได้ง่ายขึ้นเลย (Perfect Secrecy)
- **Plausible Deniability (ป้องกัน $5 Wrench Attack):** แต่ละชิ้นส่วน **เป็น BIP-39 seed ที่ใช้งานได้จริง** สามารถโอน Bitcoin เล็กน้อยเข้าไปทำเป็น "กระเป๋าหลอก" ถ้าโดนบังคับ ส่งมอบชิ้นส่วนให้ไป — โจรไม่มีทางรู้ว่านั่นคือแค่ครึ่งหนึ่งของกุญแจ

---

### ⚔️ เปรียบเทียบวิธีปกป้อง Seed: Multisig vs. Shamir Backup vs. Seed XOR

มีสามวิธีหลักในการกระจายความเสี่ยงของ Bitcoin seed ให้ไม่มีจุดเดียวที่ถ้าพังแล้วสูญทุกอย่าง แต่ละวิธีแก้ปัญหาเดียวกันด้วยแนวทางที่แตกต่างกัน มีข้อดีข้อเสียต่างกัน และมีความซับซ้อนต่างระดับกัน

#### 🔑 ภาพรวมเปรียบเทียบ

| เกณฑ์ | Multisig | Shamir Backup (SLIP-39) | Seed XOR (BIP-39) |
|---|---|---|---|
| มาตรฐาน | Bitcoin Script / BIP-11 | SLIP-39 (Satoshi Labs) | เข้ากันได้กับ BIP-39 |
| รูปแบบ | M-of-N (เช่น 2-of-3) | M-of-N (เช่น 2-of-3) | M-of-M (ต้องครบทุกชิ้น) |
| กู้คืนได้แม้ขาดบางส่วน | ✅ ได้ (M < N) | ✅ ได้ (M < N) | ❌ ไม่ได้ (ต้องครบทุกชิ้น) |
| Hardware wallet รองรับ | ✅ กว้างขวาง (Trezor, Coldcard, Ledger) | ⚠️ จำกัด (Trezor Model T/Safe) | ✅ ทุก BIP-39 wallet |
| ต้องใช้ tool พิเศษในการกู้คืน | ✅ Wallet software | ✅ SLIP-39 tool | ✅ XOR calculator (tool นี้) |
| Plausible deniability | ❌ ไม่มี | ❌ Shares ดูออกว่าเป็น share | ✅ แต่ละ share ดูเหมือน seed จริง |
| มีหลักฐานทางคณิตศาสตร์รองรับ | ✅ Shamir's Secret Sharing | ✅ Shamir's Secret Sharing | ✅ Information-theoretic |
| ความซับซ้อน | 🔴 สูงมาก | 🟡 ปานกลาง | 🟢 ต่ำ |
| จุดเดียวที่ถ้าพังสูญทุกอย่าง | ✅ ไม่มี | ✅ ไม่มี | ⚠️ ไม่มี (เฉพาะเมื่อ M=N) |
| เปลี่ยน on-chain footprint | 🔴 ใช่ (format address เปลี่ยน) | ❌ ไม่ | ❌ ไม่ |
| ใช้กับ seed เดิมที่มีอยู่แล้วได้ | ❌ ต้องตั้ง wallet ใหม่ | ❌ ต้อง generate seed ใหม่ | ✅ ใช้กับ seed เดิมได้ทันที |

---

#### 1️⃣ Multisig (Multi-Signature / หลายลายเซ็น)

**วิธีทำงาน:** แทนที่จะใช้ private key เดียวควบคุม wallet Multisig กำหนดให้ต้องมี **M ลายเซ็นจาก N keys ที่ต่างกัน** จึงจะอนุมัติ transaction ได้ ตัวอย่าง: 2-of-3 หมายถึง ต้องใช้ key อย่างน้อย 2 จาก 3 ตัว โดยแต่ละ key เป็น device หรือ seed คนละชุดกันอย่างอิสระ

**ข้อดี:**
- **Threshold M-of-N จริง:** ในระบบ 2-of-3 ถ้าเสีย key 1 ตัวยังกู้คืนได้ด้วย 2 ตัวที่เหลือ
- **ความปลอดภัยพิสูจน์ได้ทางคณิตศาสตร์** ระดับ transaction — device เดียวถูกแฮ็กไม่สูญเงิน
- **ผ่านการใช้งานจริงมาแล้ว** — ใช้โดย exchange, สถาบัน, และ Bitcoiners ขั้นสูงมาหลายปี
- **ผู้โจมตีต้องเจาะหลาย device พร้อมกัน** ทำได้ยากมากในทางปฏิบัติ

**ข้อเสีย:**
- **ซับซ้อนมาก:** ต้องจัดการ hardware wallet หลายตัว, xpubs, และ wallet descriptors อย่างระมัดระวัง ผิดพลาดในการ backup อาจล็อก fund ถาวร
- **เปลี่ยน on-chain footprint:** Multisig ใช้ address แบบ P2SH หรือ P2WSH ซึ่งต่างจาก P2WPKH — บน blockchain เห็นออกว่าเป็น Multisig
- **ใช้กับ seed เดิมไม่ได้** — ต้อง migrate wallet ทั้งหมดไปยังระบบใหม่
- **ทายาทหรือคนกู้เงินต้องเข้าใจวิธีรวม key** และใช้ wallet software ที่ถูกต้อง
- **ไม่มี plausible deniability:** การมี Multisig wallet เห็นออกชัดเจนบน blockchain
- **ความเสี่ยงจาก vendor:** ถ้า hardware wallet รุ่นนั้นหยุดผลิตหรือ software เปลี่ยน อาจมีปัญหาในการกู้คืน

**เหมาะสำหรับ:** การเก็บ Bitcoin มูลค่าสูงมาก, สถาบัน, หรือผู้ใช้ที่มีความรู้ด้านเทคนิคและยอมรับความซับซ้อน

---

#### 2️⃣ Shamir's Secret Sharing / Shamir Backup (SLIP-39)

**วิธีทำงาน:** ใช้อัลกอริทึม **Shamir's Secret Sharing (SSS)** แบ่ง secret ออกเป็น N ส่วน โดยนำ M ส่วนใดก็ได้มารวมกันเพื่อกู้คืน SSS ทำงานด้วยการ interpolation polynomial บน finite field ซึ่งต่างจาก XOR ตรงที่ต้องการแค่ M-of-N ไม่ใช่ทั้งหมด

**ข้อดี:**
- **M-of-N threshold จริง (M < N):** ตัวอย่าง 2-of-5 ใช้แค่ 2 จาก 5 share ก็กู้คืนได้ — มี redundancy แท้จริง ถ้าหาย 1 share ยังไม่สูญเงิน
- **พิสูจน์ทางคณิตศาสตร์แล้ว:** อิงหลักการ Shamir's Secret Sharing ที่ได้รับการยอมรับในวงการวิชาการ
- **Hardware wallet รองรับ:** Trezor Model T และ Trezor Safe รองรับ SLIP-39 แบบ native
- **ไม่มีจุดล้มเหลวเดี่ยว** ด้วย redundancy แท้จริง (M < N)

**ข้อเสีย:**
- **Hardware support จำกัด:** ส่วนใหญ่เป็น Trezor เท่านั้น Coldcard, Ledger, และ hardware wallet อื่นๆ ส่วนใหญ่ไม่รองรับ SLIP-39 ณ ปี 2025
- **ใช้กับ BIP-39 seed เดิมไม่ได้:** SLIP-39 สร้าง seed ต่างรูปแบบ — ต้อง generate ใหม่และย้าย fund ทั้งหมด
- **Shares ดูออกว่าเป็น share:** SLIP-39 share ไม่เหมือน BIP-39 seed ปกติ — ไม่มี plausible deniability
- **ต้องใช้ software ที่รองรับ SLIP-39** ในการ reconstruct ซึ่งไม่ได้มีทั่วไปเหมือน BIP-39
- **ต้องไว้ใจ device ตอน generate:** ถ้า Trezor ถูก compromise ตอนแบ่ง shares — ทุก share ถูก compromise

**เหมาะสำหรับ:** ผู้ใช้ Trezor ที่ต้องการ M-of-N threshold แบบแท้จริง และยินดีเริ่มต้นด้วย seed ใหม่

---

#### 3️⃣ Seed XOR (BIP-39 XOR)

**วิธีทำงาน:** ใช้ **Bitwise XOR** กับ entropy bits ดิบของ BIP-39 seed phrase แบ่ง: สร้าง Share A แบบสุ่มและ Share B โดย `A XOR B = Master Seed` กู้คืน: `Share A XOR Share B = Master Seed` แต่ละ share เป็น BIP-39 seed ที่ใช้ได้จริงอิสระ XOR preserves entropy ทุก bit อย่างสมบูรณ์ — ความปลอดภัยเป็นแบบ information-theoretic (พิสูจน์แล้วว่าเป็นไปไม่ได้ที่จะ reconstruct โดยไม่มีครบทุก share)

**ข้อดี:**
- **ใช้กับ BIP-39 seed เดิมที่มีอยู่ได้ทันที:** ไม่ต้อง migrate แบ่ง seed ปัจจุบันได้เลยโดยไม่ต้องสร้าง wallet ใหม่
- **ทุก BIP-39 wallet ตรวจสอบ share ได้:** โหลด share ใดก็ได้เข้า Trezor, Coldcard, Ledger เพื่อยืนยันว่าเปิด (decoy) wallet ได้จริง
- **Plausible deniability:** แต่ละ share แยกไม่ออกจาก seed จริง โอน BTC เล็กน้อยเข้า share — ส่งมอบถ้าโดนบังคับ โดยที่โจรไม่รู้ว่านั่นคือแค่ครึ่งหนึ่ง
- **ไม่เปลี่ยน on-chain footprint:** Master Seed ยังสร้าง address เดิม ไม่ต้อง migrate wallet
- **คณิตศาสตร์ง่ายและตรวจสอบได้:** XOR เป็นการดำเนินการ binary ที่ง่ายที่สุด ตรวจสอบด้วยมือหรือ tool open-source นี้ได้
- **ไม่พึ่งพา vendor:** ไม่มี format เฉพาะ ใช้ได้กับ open standard ตลอดไป
- **Information-theoretic security:** พิสูจน์แล้วว่าการรู้ M-1 shares ไม่เปิดเผยข้อมูลเกี่ยวกับ master seed เลยแม้แต่บิตเดียว

**ข้อเสีย:**
- **M-of-M เท่านั้น (ไม่มี threshold):** All-or-Nothing ถ้าสร้าง 3 shares แล้วหาย 1 อัน — fund สูญถาวร ไม่มีระบบ "ขอแค่ 2-of-3 ก็พอ"
- **กระบวนการด้วยมือต้องการ tool ที่เชื่อถือได้:** ต้องใช้ XOR calculator ที่ถูกต้อง tool ที่มีบั๊ก (เช่น ที่ไม่ recompute checksum ของคำสุดท้าย) จะสร้าง Master Seed ที่ผิดแต่ดูเหมือนถูก — เปิดผิด wallet
- **ไม่มี hardware wallet รองรับการแบ่ง natively:** การแบ่ง share ต้องทำภายนอก (tool นี้) hardware wallet จัดการแค่ individual BIP-39 shares ในฐานะ seed ปกติ
- **Coldcard รองรับ XOR:** Coldcard firmware มีฟีเจอร์ XOR native — แต่ hardware wallet อื่นๆ ส่วนใหญ่ไม่มีฟังก์ชัน combine XOR ในตัว

**เหมาะสำหรับ:** ผู้ถือ Bitcoin self-custody ที่ต้องการแบ่ง seed เดิมเก็บหลายที่ ให้ความสำคัญกับความเรียบง่ายและการตรวจสอบได้ ต้องการ plausible deniability และเข้าใจ trade-off ของ all-or-nothing

---

#### 🎯 คู่มือตัดสินใจ: ควรเลือกวิธีไหน?

```
ต้องการแบ่ง seed เดิมที่มีอยู่โดยไม่ต้องสร้าง wallet ใหม่?
├── ใช่ → Seed XOR คือตัวเลือกเดียวของคุณ
└── ไม่ (เริ่มต้นใหม่)
    ├── ต้องการ M-of-N threshold (ขาดบาง share แต่ยังกู้คืนได้)?
    │   ├── ใช่ → Shamir Backup (ถ้ามี Trezor) หรือ Multisig
    │   └── ไม่ (ยอมรับ all-or-nothing ได้) → Seed XOR
    ├── ต้องการ plausible deniability (ป้องกัน $5 Wrench Attack)?
    │   ├── ใช่ → Seed XOR
    │   └── ไม่ → วิธีไหนก็ได้
    └── ต้องการความปลอดภัยระดับสถาบันด้วย device อิสระหลายตัว?
        ├── ใช่ → Multisig
        └── ไม่ → Seed XOR หรือ Shamir
```

> **คำตอบตรงๆ:** สำหรับผู้ถือ Bitcoin รายบุคคลส่วนใหญ่ **Seed XOR คือสมดุลที่ดีที่สุดระหว่างความปลอดภัย ความเรียบง่าย และความเข้ากันได้** — โดยมีข้อแม้สำคัญที่ต้องเข้าใจและยอมรับ trade-off ของ all-or-nothing

---

### ✨ คุณสมบัติเด่น

- **ออฟไลน์ 100%** — ไม่มีการเรียกข้อมูลจากภายนอก ไฟล์เดียวมีทุกอย่างในตัว
- **BIP-39 Wordlist 2,048 คำ** — ฝังไว้ในไฟล์ตรงๆ ไม่ต้องโหลดเพิ่ม
- **SHA-256 engine ในตัว** — เขียนด้วย Pure JavaScript ตาม FIPS 180-4 ไม่พึ่ง Web Crypto API
- **Checksum-aware XOR** — คำนวณ checksum ของคำสุดท้ายถูกต้องตาม BIP-39 (ไม่ใช่แค่ XOR ทื่อๆ)
- **รองรับหลายขนาด** — 12, 15, 18, 21, 24 คำ และ 2–10 shares
- **Autocomplete** — พิมพ์ prefix แล้วมีคำขึ้นให้เลือก, Tab/Enter เพื่อยืนยัน
- **Paste mode** — วาง seed ทั้งชุดในครั้งเดียวได้เลย
- **ตาราง XOR ระดับ bit** — แสดง Binary 11-bit, Decimal, และ คำ ทุกขั้นตอน ตรวจสอบเองได้
- **Hard Stop** — ปุ่มคำนวณถูก lock ถ้า checksum ของ share ใดผิด
- **ตรวจ environment** — ตรวจจับ remote server และ block โดยอัตโนมัติ (มีปุ่ม Demo Mode สำหรับการศึกษา)
- **Clear Session** — ปุ่มล้างข้อมูลทั้งหมดออกจาก RAM ด้วย `location.reload()`

---

### 🧮 หลักการทำงาน BIP-39 XOR (ด้านเทคนิค)

```
12 คำ = 132 bits = 128 bits entropy + 4 bits checksum
24 คำ = 264 bits = 256 bits entropy + 8 bits checksum

คำที่ 1–11  : 11 bits แต่ละคำ → entropy ล้วน → XOR ปกติ
คำที่ 12    : 7 bits entropy (XOR) + 4 bits checksum (คำนวณใหม่จาก SHA-256)

คำที่ 1–23  : 11 bits แต่ละคำ → entropy ล้วน → XOR ปกติ
คำที่ 24    : 3 bits entropy (XOR) + 8 bits checksum (คำนวณใหม่จาก SHA-256)
```

Checksum ของ Master Seed **คำนวณใหม่เสมอจาก entropy ที่ XOR แล้ว** — ไม่ใช่นำ checksum ของแต่ละ share มา XOR กัน

SHA-256 self-test รันทุกครั้งที่เปิดหน้าเพื่อยืนยันว่า engine ทำงานถูกต้องก่อนคำนวณ

---

### 📥 วิธีดาวน์โหลดและใช้งาน (Secure Mode)

> ⚠️ **ห้ามป้อน Seed จริงบนเครื่องที่ต่ออินเทอร์เน็ตเด็ดขาด**

1. **ดาวน์โหลด:** คลิก `Seed-XOR.html` → **Raw** → `Ctrl+S` บันทึกไฟล์
2. **ตรวจสอบ checksum** (ดูส่วน Checksums ด้านล่าง) — ยืนยันว่าไฟล์ไม่ถูกดัดแปลง
3. คัดลอกไฟล์ไปยัง USB ที่สะอาด
4. Boot เข้า **Tails OS** หรือ OS ออฟไลน์
5. **ถอดสาย LAN และปิด Wi-Fi อย่างเด็ดขาด**
6. เปิดไฟล์ `Seed-XOR.html` ในเบราว์เซอร์
7. ยืนยันว่า SHA-256 self-test แสดง **สีเขียว** ก่อนเริ่มป้อนข้อมูล
8. ป้อน shares, กดคำนวณ, จด Master Seed ลงกระดาษหรือแผ่นเหล็ก
9. กด **🔥 ล้างเซสชันทั้งหมด** เมื่อเสร็จแล้ว
10. **ปิดเครื่องทันที** — RAM ถูกล้างเมื่อปิดไฟ

---

### 🔐 การตรวจสอบความถูกต้องของไฟล์ (Checksums)

ตรวจสอบก่อนใช้ทุกครั้ง เพื่อยืนยันว่าไฟล์ไม่ถูกดัดแปลงหรือฝังมัลแวร์

**ชื่อไฟล์:** `Seed-XOR.html`

| Algorithm | Hash |
|---|---|
| **SHA-256** | `00fa0af4997d0ace1cd4ec57cf7be906e8d80de8eaca335716576d5a7397bcae` |
| **MD5** | `b778e002d05353476d5ecfc503172db9` |

#### วิธีตรวจสอบบนระบบปฏิบัติการต่างๆ:

**Windows (PowerShell)**
```powershell
Get-FileHash .\Seed-XOR.html -Algorithm SHA256
Get-FileHash .\Seed-XOR.html -Algorithm MD5
```

**macOS (Terminal)**
```bash
shasum -a 256 Seed-XOR.html
md5 Seed-XOR.html
```

**Linux / Tails OS (Terminal)**
```bash
sha256sum Seed-XOR.html
md5sum Seed-XOR.html
```

---

### 🔒 Security Model (ภาษาไทย)

- **ไม่มีการเรียกเครือข่าย** — ตรวจสอบ source code ได้: ไม่มี `fetch()`, `XMLHttpRequest`, หรือ `<script src>` ภายนอก
- **ไม่ใช้ localStorage** — ข้อมูลทั้งหมดอยู่ใน memory เท่านั้น ปิดหน้าต่าง = ล้างทันที
- **Input sanitization** — ทุก input ผ่าน BIP-39 wordlist filter ก่อน render → ไม่มีช่องโหว่ XSS
- **Hard stop** — ถ้า share ใดมี checksum ผิด ปุ่มคำนวณจะถูก disable ทันที ไม่มีปุ่ม "ยืนยันทำต่อ"
- **Defense in depth** — OPSEC lock ทำงาน 3 ชั้น: `IS_UNSAFE_ENV`, `updateCalcBtn()`, และ `calculate()`

---

### 🤝 เครดิต

- **ผู้พัฒนา:** [Chollatis Bitcoiner](https://github.com/chontit) — นักบินกองทัพอากาศและนักการศึกษา Bitcoin ชุมชน Korat Bitcoiner
- **AI Co-pilot:** Claude (Anthropic) — สถาปัตยกรรม, โค้ด, ความปลอดภัย
- **Security Audit:** Gemini (Google) — ตรวจสอบโค้ด 4 รอบ, ค้นพบ edge cases
- **แพลตฟอร์มการศึกษา Bitcoin:** [learning.chontit.win](https://learning.chontit.win)
- **BIP-39 Wordlist:** [trezor/python-mnemonic](https://github.com/trezor/python-mnemonic)
- **มาตรฐานอ้างอิง:** [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) · [FIPS 180-4](https://csrc.nist.gov/publications/detail/fips/180/4/final)

---

*Don't Trust, Verify ⚡*
