# Instructor Runbook: Password Cracking Demo (John the Ripper)

**Duration:** ~45–60 minutes (theory + hands-on)  
**Last prepared:** 2025-08-18

## 1) Learning Goals
- Understand hashes vs plaintext passwords
- Observe how quickly weak passwords fall to dictionary attacks
- Discuss defenses: salting, slow KDFs (bcrypt/scrypt/Argon2), MFA, lockouts, rate limiting

## 2) Files to Hand Out
- `md5_hashes.txt` — 10 MD5 hashes
- `sha1_hashes.txt` — 10 SHA-1 hashes
- `class_wordlist.txt` — small dictionary
- (Optional) Provide a link to a larger wordlist, e.g., `rockyou.txt` if students have it locally.

## 3) Setup Checks (2–5 min)
```bash
john --list=build-info
```
Explain jumbo build features, formats, and GPU support (if any).

## 4) Live Demo Script (20–30 min)
### A) MD5 Dictionary Attack
```bash
john --format=raw-MD5 --wordlist=class_wordlist.txt md5_hashes.txt
john --show --format=raw-MD5 md5_hashes.txt
```
**Talking points:**
- Show how many c/s (candidates per second) appear.
- Emphasize that the *same* password always has the *same* MD5 hash (no salt) → easy for attackers to reuse work and rainbow tables.

### B) SHA-1 Dictionary Attack
```bash
john --format=raw-SHA1 --wordlist=class_wordlist.txt sha1_hashes.txt
john --show --format=raw-SHA1 sha1_hashes.txt
```
**Talking points:**
- SHA-1 is stronger than MD5 in collision resistance, but both are **fast** → bad for password hashing.
- Make the contrast with **slow** password hashing: bcrypt, scrypt, Argon2.

### C) Rules (Optional, +5 min)
```bash
john --format=raw-MD5 --wordlist=class_wordlist.txt --rules=Single md5_hashes.txt
john --show --format=raw-MD5 md5_hashes.txt
```
Explain how rules try more candidates by mutating the dictionary (e.g., `password` → `Password1`).

### D) Bonus Challenge (Optional)
Give students one extra MD5 hash and see if they can crack it within 2–3 minutes using rules or a bigger wordlist.

## 5) Discussion & Defensive Controls (10–15 min)
- **Never** store plaintext; always store **salted** and **slow-hashed** passwords (bcrypt/Argon2id).
- Rate limiting, MFA, password length limits (low minimum, high maximum), breach detection.
- User education: password managers, unique passwords per site.

## 6) Troubleshooting
- If `No password hashes loaded` → wrong `--format` or bad file path.
- If it’s “too fast” and ends instantly: hashes already in the pot file. Clear with:
  ```bash
  rm ~/.john/john.pot
  ```
- On Windows, prefer **WSL** (Ubuntu) for a smoother CLI experience.
- To monitor progress during a run, press `s` in the terminal or run:
  ```bash
  john --status
  ```

## 7) Safety & Ethics Slide (read aloud)
- Test only what you own or have permission for.
- Use demo data in this pack only.
- Don’t run scans/cracking tools on school networks without authorization.
