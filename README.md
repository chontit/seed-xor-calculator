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

**Available in two standalone versions: English (`Seed-XOR_english.html`) and Thai (`Seed-XOR_thai.html`).**

Co-developed with Claude AI · Audited by Gemini AI · Built by [@chontit](https://github.com/chontit)

---

### ❓ What is Seed XOR?

Seed XOR is an advanced cryptographic method that uses **Bitwise Exclusive OR (XOR)** to mathematically split a single Master Seed into multiple independent "Shares". The Master Seed can only be reconstructed when **all** shares are combined.

Unlike the dangerous practice of physically cutting a paper seed backup in half (which drastically reduces entropy and leaks partial information), Seed XOR provides **mathematically perfect secrecy**:

| Feature | Seed XOR | Cut Paper in Half ✂️ |
|---|---|---|
| **Entropy Preserved** | ✅ Yes (Full 256-bit security) | ❌ No (~128-bit per half, easily brute-forced) |
| **Recovery Logic** | ✅ All-or-Nothing | ❌ Incomplete & vulnerable |
| **Decoy Potential** | ✅ Yes (Each share is a valid wallet) | ❌ Obviously a torn piece of paper |
| **Auditability** | ✅ Open, verifiable math | ❌ None |

**Key Properties of Seed XOR:**
1. **All-or-Nothing (M-of-M):** If you split a seed into 3 shares, you absolutely need all 3 to recover it. Losing 1 share means the funds are permanently inaccessible. An attacker finding 2 out of 3 shares gains **zero** mathematical advantage in guessing the final share.
2. **Plausible Deniability ($5 Wrench Attack Protection):** The magic of Seed XOR is that **each individual share is a 100% valid BIP-39 seed phrase on its own**. You can deposit a small amount of Bitcoin into each share to act as "Decoy Wallets". If physically threatened, you can surrender a single share, and the attacker will have no cryptographic way to prove it's only a fraction of your true wealth.

---

### ⚔️ Seed Backup Methods Compared: Multisig vs. Shamir vs. Seed XOR

| Criteria | 1️⃣ Multisig | 2️⃣ Shamir (SLIP-39) | 3️⃣ Seed XOR (This tool) |
|---|---|---|---|
| **Structure** | M-of-N keys signing a transaction | M-of-N shares to recreate a seed | M-of-M shares to recreate a seed |
| **Threshold Recovery** | ✅ Yes (Lose a key, still safe) | ✅ Yes (Lose a share, still safe) | ❌ No (Lose a share, lose all funds) |
| **Plausible Deniability**| ❌ No (Visible on-chain) | ❌ No (Words identify as shares) | ✅ **Yes (Shares look like real seeds)** |
| **Use Existing Seed?** | ❌ No (Requires new setup) | ❌ No (Requires new SLIP-39 setup) | ✅ **Yes (Works with any existing seed)** |
| **Complexity Level** | 🔴 High (Requires xpub/descriptor management) | 🟡 Medium | 🟢 **Low (Just seed words)** |

---

### ✨ Tool Features

* **100% Offline & Air-gapped Ready:** Single `.html` file. No CDN, no external scripts.
* **Embedded BIP-39 Wordlist:** All 2,048 words are built-in.
* **Native SHA-256 Engine:** Pure JavaScript FIPS 180-4 implementation. Does not rely on Web Crypto API (ensuring compatibility across all offline environments).
* **Checksum-Aware XOR:** Correctly calculates the 8-bit checksum for the final 24th word (it does not just naively XOR the last word).
* **Flexible Configurations:** Supports 12 to 24-word seeds, and 2 to 10 shares.
* **Transparent Verification:** Displays a bit-by-bit XOR table so you can verify the math manually.
* **Strict OPSEC Lock:** Automatically blocks execution if opened on a networked server (`http/https`).

---

### 📥 Secure Usage Guide

> ⚠️ **NEVER enter your real seed phrase on an internet-connected device.**

1. **Download:** Choose your preferred language file (`Seed-XOR_english.html` or `Seed-XOR_thai.html`). Right-click → **Save link as...**
2. **Verify Checksum:** Ensure the file hash matches the values provided below.
3. **Go Offline:** Transfer the file to a USB drive and boot into **Tails OS** (or a clean, offline computer). Physically unplug the LAN cable and turn off Wi-Fi.
4. **Calculate:** Open the file, enter your shares, and calculate your Master Seed.
5. **Secure:** Write your Master Seed on paper or metal. Do not take photos.
6. **Wipe:** Click **🔥 Clear Session** and shut down the computer immediately to wipe the RAM.

---

### 🔐 File Verification (Checksums)

To ensure the files have not been tampered with or intercepted, verify the checksums before use:

**1. English Version (`Seed-XOR_english.html`)**
| Algorithm | Hash |
|---|---|
| **SHA-256** | `7bd489209872166db1b2aa87945f184a9d2c8c7bccccc56ba0ffbd78f5ba7b6d` |
| **MD5** | `58746ad8203a400dd26dae0ee257bd37` |

**2. Thai Version (`Seed-XOR_thai.html`)**
| Algorithm | Hash |
|---|---|
| **SHA-256** | `00fa0af4997d0ace1cd4ec57cf7be906e8d80de8eaca335716576d5a7397bcae` |
| **MD5** | `b778e002d05353476d5ecfc503172db9` |

**Verification Commands:**
* **Windows (PowerShell):** `Get-FileHash .\<filename>.html -Algorithm SHA256`
* **macOS / Linux:** `shasum -a 256 <filename>.html`

---

## 🇹🇭 ภาษาไทย

เครื่องมือคำนวณ **Seed XOR** สำหรับมาตรฐาน BIP-39 แบบออฟไลน์ 100% ออกแบบมาเพื่อความปลอดภัยสูงสุดระดับ Cypherpunk (OPSEC ระดับสูงสุด) เหมาะสำหรับใช้งานบนระบบที่ถูกตัดขาดจากอินเทอร์เน็ต (Air-gapped) เช่น Tails OS

**โปรเจกต์นี้มีให้เลือกใช้งาน 2 ภาษาในรูปแบบไฟล์เดียวจบ: ภาษาอังกฤษ (`Seed-XOR_english.html`) และภาษาไทย (`Seed-XOR_thai.html`)**

พัฒนาร่วมกับ Claude AI · ตรวจสอบความปลอดภัยโดย Gemini AI · สร้างโดย [@chontit](https://github.com/chontit)

---

### ❓ Seed XOR คืออะไร?

**Seed XOR** คือเทคนิคทางวิทยาการเข้ารหัสลับ (Cryptography) ที่ใช้คณิตศาสตร์ระดับบิตที่เรียกว่า **Exclusive OR (XOR)** เพื่อ "ตอกรหัสทับกัน" หรือพูดง่ายๆ คือการแยก Seed หลัก (Master Seed) ออกเป็นหลายๆ ชิ้นส่วน (Shares) โดยคุณจะสามารถเข้าถึงเงินได้ก็ต่อเมื่อนำชิ้นส่วน **"ทั้งหมด"** มารวมร่างกันเท่านั้น

**ทำไมถึงห้าม "ตัดกระดาษจด Seed แบ่งครึ่ง"?**
หลายคนมักเอา Seed 24 คำมาตัดกระดาษแบ่งครึ่ง (แผ่นละ 12 คำ) ไปซ่อนคนละที่ ซึ่งเป็นวิธีที่ **อันตรายมาก** เพราะถ้าโจรได้กระดาษไป 1 แผ่น โจรจะเดาคำที่เหลือได้ง่ายขึ้นมหาศาล แต่ Seed XOR แก้ปัญหานี้ได้อย่างสมบูรณ์แบบ:

| คุณสมบัติ | เทคนิค Seed XOR 🛡️ | การตัดกระดาษแบ่งครึ่ง ✂️ |
|---|---|---|
| **ความปลอดภัย (Entropy)** | ✅ ปลอดภัยเต็ม 256-bit เท่าเดิม | ❌ ความปลอดภัยลดลงครึ่งหนึ่ง โดนแฮ็กได้ง่าย |
| **เงื่อนไขการกู้คืน** | ✅ ต้องมีครบทุกชิ้นเท่านั้น (All-or-Nothing) | ❌ ข้อมูลแหว่งไปครึ่งหนึ่ง |
| **การพรางตัว (Decoy Wallet)**| ✅ ทำได้ (แต่ละชิ้นส่วนดูเหมือน Seed ปกติ) | ❌ ทำไม่ได้ (โจรเห็นก็รู้ว่ากระดาษโดนฉีก) |
| **การตรวจสอบย้อนกลับ** | ✅ ตรวจสอบสมการคณิตศาสตร์ได้ | ❌ ไม่มีหลักการทางคณิตศาสตร์รองรับ |

**จุดเด่นที่แท้จริงของ Seed XOR:**
1. **ทฤษฎีความลับสมบูรณ์แบบ (Perfect Secrecy):** สมมติคุณแบ่ง Seed เป็น 3 ชิ้น (A, B, C) หากแฮ็กเกอร์ขโมยชิ้น A และ B ไปได้ ข้อมูลที่แฮ็กเกอร์มีจะไม่ช่วยให้เดาชิ้น C ได้ง่ายขึ้นเลยแม้แต่นิดเดียว (ข้อมูลที่หลุดไปมีค่าเท่ากับศูนย์)
2. **สุดยอดการพรางตัว (Plausible Deniability):** ชิ้นส่วนย่อยแต่ละชิ้นของ Seed XOR **เป็น Seed 24 คำที่สามารถนำไปเปิดกระเป๋าใช้งานได้จริง!** คุณสามารถโอน Bitcoin จำนวนเล็กน้อยเข้าไปเก็บไว้เพื่อทำเป็น "กระเป๋าล่อเป้า (Decoy)" ได้ หากวันหนึ่งคุณถูกโจรเอาปืนจ่อหัวข่มขู่ ($5 Wrench Attack) คุณสามารถส่งมอบชิ้นส่วนย่อยให้โจรไปได้เลย โดยที่โจรจะเปิดเจอเงินเล็กน้อยและคิดว่าปล้นสำเร็จแล้ว โดยไม่มีทางรู้เลยว่านั่นคือกลไกซ่อนตู้เซฟใบใหญ่ของคุณ

---

### ⚔️ เปรียบเทียบวิธีจัดเก็บ Seed ขั้นสูง (Multisig vs Shamir vs Seed XOR)

| เกณฑ์การพิจารณา | 1️⃣ Multisig (หลายลายเซ็น) | 2️⃣ Shamir Backup (SLIP-39) | 3️⃣ Seed XOR (เครื่องมือนี้) |
|---|---|---|---|
| **โครงสร้างระบบ** | ใช้หลายกุญแจเซ็นเพื่อโอนเงิน | ตัดแบ่ง Seed ออกเป็นหลายชิ้น | นำ Seed มาทับซ้อนกันหลายชั้น |
| **ความยืดหยุ่น (Threshold)** | ✅ ยืดหยุ่น (เช่น มี 3 หาย 1 ยังกู้ได้) | ✅ ยืดหยุ่น (เช่น มี 5 หาย 2 ยังกู้ได้)| ❌ บังคับใช้ครบ (หาย 1 ชิ้นคือเงินสูญถาวร)|
| **การพรางตัวหลอกโจร** | ❌ ไม่มี (ทิ้งร่องรอยไว้บน Blockchain) | ❌ ไม่มี (คำศัพท์บอกชัดเจนว่าเป็นชิ้นส่วน)| ✅ **สมบูรณ์แบบ (ชิ้นส่วนเนียนเป็นกระเป๋าปกติได้)**|
| **การใช้กับ Seed เก่าที่มีอยู่** | ❌ ต้องสร้าง Wallet ใหม่ทั้งหมด | ❌ ต้องสร้าง Seed มาตรฐานใหม่ | ✅ **ใช้กับ Seed เดิมของคุณที่มีอยู่แล้วได้ทันที**|
| **ความซับซ้อนในการจัดการ**| 🔴 สูงมาก (ต้องสำรองไฟล์ Descriptor ให้ดี)| 🟡 ปานกลาง | 🟢 **ต่ำ (จัดการแค่คำศัพท์บนกระดาษ)** |

---

### ✨ คุณสมบัติของโปรแกรมนี้ (Features)

* **ออฟไลน์ 100% ระดับสุดยอด:** ไม่มี External Links, ไม่มีการดึงข้อมูลจาก CDN ไฟล์ทุกอย่างเขียนจบในไฟล์ `.html` เพียงไฟล์เดียว
* **ระบบประมวลผล SHA-256 ในตัว:** เขียนด้วย Pure JavaScript ตามมาตรฐาน FIPS 180-4 ไม่พึ่งพาระบบของเบราว์เซอร์ เพื่อให้แน่ใจว่ารันออฟไลน์ได้บนทุกเครื่อง
* **คำนวณ Checksum แม่นยำ:** เครื่องมือนี้ไม่ได้จับคำที่ 24 มา XOR กันทื่อๆ แต่จะสกัด Entropy บิตสุดท้าย และนำไปเข้าสมการ SHA-256 เพื่อสร้าง Checksum ตัวใหม่ที่ถูกต้องตามมาตรฐาน BIP-39 เป๊ะๆ
* **โปร่งใส ตรวจสอบได้ (Verify):** มีตารางแสดงผลลัพธ์ Binary แบบ 11-bit ของแต่ละคำให้คุณดูว่าระบบประมวลผลคณิตศาสตร์อย่างไร
* **ระบบล็อคความปลอดภัย (OPSEC Lock):** หากมีคนพยายามเปิดไฟล์นี้บนเซิร์ฟเวอร์อินเทอร์เน็ต (`http/https`) ระบบจะขึ้นป้ายเตือนสีแดงและล็อคปุ่มคำนวณทันที ป้องกันผู้ใช้เผลอนำ Seed จริงมาป้อนออนไลน์ (แต่มีปุ่มให้กดปลดล็อคเพื่อใช้เป็นกรณีศึกษาได้)

---

### 📥 วิธีดาวน์โหลดและใช้งาน (ปลอดภัยสูงสุด)

> ⚠️ **กฎเหล็ก: ห้ามป้อน Seed จริงบนคอมพิวเตอร์ที่ต่ออินเทอร์เน็ต หรือเครื่องที่ใช้ทำงานประจำเด็ดขาด**

1. **ดาวน์โหลด:** เลือกไฟล์ในภาษาที่คุณต้องการ (`Seed-XOR_english.html` หรือ `Seed-XOR_thai.html`) บน GitHub นี้ กดคลิกขวาที่ไฟล์ แล้วเลือก **Save link as...** ลงในคอมพิวเตอร์
2. **ตรวจสอบความบริสุทธิ์ของไฟล์:** ทำการเช็คค่า SHA-256 Hash ของไฟล์ (ดูวิธีด้านล่าง) ว่าตรงกันหรือไม่ เพื่อป้องกันมัลแวร์
3. **เตรียมคอมพิวเตอร์:** ก๊อปปี้ไฟล์ใส่แฟลชไดร์ฟ บูตคอมพิวเตอร์ของคุณด้วย **Tails OS** (ระบบปฏิบัติการสำหรับสายลับที่ไม่จำข้อมูล)
4. **ตัดขาดโลกภายนอก (Air-gapped):** ถอดสาย LAN ดึงปลั๊ก Wi-Fi ออกจากคอมพิวเตอร์
5. **คำนวณ:** เปิดไฟล์ HTML กรอกชิ้นส่วน Seed และกดคำนวณ Master Seed
6. **จดบันทึก:** จด Master Seed ลงบนกระดาษหรือตอกลงแผ่นเหล็กให้เรียบร้อย (ห้ามถ่ายรูปหน้าจอเด็ดขาด)
7. **ทำลายหลักฐาน:** กดปุ่ม **🔥 ล้างเซสชันทั้งหมด** บนหน้าจอ จากนั้นกด **Shut down (ปิดเครื่อง)** ทันที ข้อมูลทั้งหมดจะระเหยหายไปจาก RAM แบบไร้ร่องรอย

---

### 🔐 การตรวจสอบความปลอดภัยของไฟล์ (Checksums)

เพื่อความสบายใจสูงสุด คุณควรตรวจสอบค่า Hash ของไฟล์ก่อนนำไปรันออฟไลน์:

**1. เวอร์ชันภาษาอังกฤษ (`Seed-XOR_english.html`)**
| อัลกอริทึม | รหัส Hash ที่ถูกต้อง |
|---|---|
| **SHA-256** | `7bd489209872166db1b2aa87945f184a9d2c8c7bccccc56ba0ffbd78f5ba7b6d` |
| **MD5** | `58746ad8203a400dd26dae0ee257bd37` |

**2. เวอร์ชันภาษาไทย (`Seed-XOR_thai.html`)**
| อัลกอริทึม | รหัส Hash ที่ถูกต้อง |
|---|---|
| **SHA-256** | `00fa0af4997d0ace1cd4ec57cf7be906e8d80de8eaca335716576d5a7397bcae` |
| **MD5** | `b778e002d05353476d5ecfc503172db9` |

**วิธีตรวจสอบบนระบบของคุณ:**
* **Windows (เปิด PowerShell):** พิมพ์คำสั่ง `Get-FileHash .\<ชื่อไฟล์>.html -Algorithm SHA256`
* **macOS / Linux / Tails OS (เปิด Terminal):** พิมพ์คำสั่ง `sha256sum <ชื่อไฟล์>.html`

---

### 🤝 เครดิตผู้พัฒนา

- **พัฒนาโดย:** [Chollatis Bitcoiner](https://github.com/chontit) — Korat Bitcoiner
- **AI Co-pilot:** ร่วมออกแบบสถาปัตยกรรมและเขียนโค้ดโดย Claude (Anthropic)
- **Security Audit:** ตรวจสอบตรรกะ ช่องโหว่ และสกัดจับ Bug โดย Gemini (Google)
- **แพลตฟอร์มการศึกษา:** [learning.chontit.win](https://learning.chontit.win)

---
*Don't Trust, Verify ⚡*
