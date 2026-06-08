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
- **BIP-39 Wordlist:** [github.com/bitcoin/bips/blob/master/bip-0039/english.txt](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)
- **มาตรฐานอ้างอิง:** [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) · [FIPS 180-4](https://csrc.nist.gov/publications/detail/fips/180/4/final)

---

*Don't Trust, Verify ⚡*
