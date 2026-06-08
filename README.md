# ⊕ Seed XOR Calculator (BIP-39)

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Production Ready](https://img.shields.io/badge/Status-Production_Ready-success)
![Security: Air-gapped](https://img.shields.io/badge/Security-Air--Gapped_Only-red)

A highly secure, 100% offline, single-file HTML tool for calculating Bitwise XOR for BIP-39 Seed Phrases. Designed with Paranoia-level OPSEC in mind.

🌐 **Live Demo (Educational Purpose Only):** [https://chontit.github.io/seed-xor-calculator/](https://chontit.github.io/seed-xor-calculator/)

---

## ✨ Features (คุณสมบัติเด่น)

* **100% Offline & Air-gapped Ready:** No external dependencies. No CDNs. No tracking. The entire logic, including the 2048 BIP-39 English wordlist, is embedded in a single HTML file.
* **Built-in SHA-256 Engine:** Accurately recalculates the 8-bit checksum for the 24th word in real-time.
* **Flexible Shares (M-of-M):** Supports combining anywhere from 2 to 10 seed shares.
* **Transparent Math (Don't Trust, Verify):** Displays bit-by-bit XOR calculations (11-bit per word) so you can mathematically verify the output.
* **Educational Demo Mode:** Prevents execution on networked servers by default, with an opt-in demo mode for learning.

---

## 🚨 CRITICAL OPSEC WARNING (ข้อควรระวังด้านความปลอดภัย)

**EN: DO NOT RUN THIS ON A NETWORK-CONNECTED MACHINE WITH REAL SEEDS.**
If you are combining real seed phrases that hold actual wealth, **you must use an air-gapped environment**. 

**TH: ห้ามป้อน Seed Phrase จริงบนอุปกรณ์ที่เชื่อมต่ออินเทอร์เน็ตโดยเด็ดขาด**
หากคุณต้องการคำนวณ Seed ที่เก็บเงินจริง โปรดปฏิบัติตามขั้นตอนความปลอดภัยขั้นสูงสุดดังนี้:

### 🛡️ How to use safely (วิธีใช้งานอย่างปลอดภัยที่สุด)
1. Download `index.html` to a USB Flash Drive. (ดาวน์โหลดไฟล์ index.html ใส่แฟลชไดร์ฟ)
2. Boot your computer using an amnesic live OS like **[Tails OS](https://tails.net/)**. (บูตคอมพิวเตอร์ด้วย Tails OS)
3. **Physically disconnect from the internet** (Turn off Wi-Fi / Unplug LAN). (ปิด Wi-Fi และถอดสายแลนออก)
4. Open `index.html` in the local browser and perform your XOR calculations. (เปิดไฟล์เพื่อคำนวณ)
5. Write down your Master Seed on paper or steel. (จด Master Seed ลงบนกระดาษหรือแผ่นเหล็ก)
6. **Shut down the computer immediately.** This completely wipes the RAM, leaving zero digital footprint. (สั่ง Shut down เครื่องทันทีเพื่อล้างข้อมูลทั้งหมดออกจาก RAM)

---

## 📖 How it Works (หลักการทำงานเบื้องหลัง)

Unlike Shamir's Secret Sharing (SSS) which uses polynomial interpolation, Seed XOR uses strict **Bitwise Exclusive OR (XOR)** logic. 

1. Each BIP-39 word corresponds to an index (0-2047) and is converted to an 11-bit binary.
2. The tool XORs the first 23 words (253 bits of entropy) bit-by-bit.
3. For the 24th word, it XORs only the remaining 3 bits of entropy (to reach 256 bits).
4. The tool ignores the original checksums, and hashes the new 256-bit entropy using `SHA-256` to generate a completely new, mathematically valid 8-bit checksum.
5. The final 3+8 bits are combined to form the 24th word of the Master Seed.

*(Note: Human calculation of the 24th word is impossible without a computer because of the SHA-256 hashing requirement).*

---

## 👨‍💻 Local Development & Audit
Anyone can audit the code by simply opening `index.html` in any text editor. It is written in pure HTML, CSS, and Vanilla JavaScript.

## 📄 License
This project is open-source and licensed under the **MIT License**. You are free to use, modify, and distribute it. 

*Stay safe, and hold your own keys.* ⚡
