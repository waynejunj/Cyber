# Student Worksheet: Password Cracking with John the Ripper

**Objective:** Learn how password hashes work, why weak passwords are unsafe, and how to ethically demonstrate cracking on a *provided* sample dataset.

> **Legal / Ethics Reminder:** Only crack hashes you own or have explicit permission to test. This exercise uses hashes generated for this class.

---

## Part A — Setup
1. Confirm John is installed:
   - **Kali/Ubuntu/Debian:** `sudo apt update && sudo apt install john -y`
   - **macOS (Homebrew):** `brew install john-jumbo`
   - **Windows (WSL Ubuntu):** Install Ubuntu in the Microsoft Store, then use the Linux command above.

2. Verify version:  
   ```bash
   john --list=build-info
   ```
   - Write the version you see here: `____________________________`

---

## Part B — Understand the Files
Your instructor gave you:
- `md5_hashes.txt` — 10 MD5 hashes of weak passwords
- `sha1_hashes.txt` — 10 SHA-1 hashes of the same passwords
- `class_wordlist.txt` — a small dictionary for a quick demo

**Q1.** What is the difference between a *hash* and a *password*?  
Your answer: ________________________________________________________________

**Q2.** Why are hashes one-way?  
Your answer: ________________________________________________________________

---

## Part C — Crack MD5 Hashes (Dictionary Attack)
1. In the folder containing the files, run:
   ```bash
   john --format=raw-MD5 --wordlist=class_wordlist.txt md5_hashes.txt
   ```
2. Watch John as it tests guesses. When it finishes, show results:
   ```bash
   john --show --format=raw-MD5 md5_hashes.txt
   ```

**Q3.** Which passwords did John crack?  
Your answer: ________________________________________________________________

**Q4.** Which ones did not crack (if any) using this wordlist?  
Your answer: ________________________________________________________________

---

## Part D — Crack SHA-1 Hashes (Dictionary Attack)
1. Run:
   ```bash
   john --format=raw-SHA1 --wordlist=class_wordlist.txt sha1_hashes.txt
   ```
2. Show the results:
   ```bash
   john --show --format=raw-SHA1 sha1_hashes.txt
   ```

**Q5.** Did SHA-1 hashes crack at a similar speed to MD5? Why/why not?  
Your answer: ________________________________________________________________

---

## Part E — Try Rule-Based Mutations (Optional)
John can mutate words (add numbers, change case, etc.). Try:
```bash
john --format=raw-MD5 --wordlist=class_wordlist.txt --rules=Single md5_hashes.txt
```
Then check:
```bash
john --show --format=raw-MD5 md5_hashes.txt
```

**Q6.** Did rules crack more passwords than the plain dictionary?  
Your answer: ________________________________________________________________

---

## Part F — Reflection
**Q7.** Based on this demo, list three reasons *long, unique* passwords are safer:  
1. __________________________________________________________  
2. __________________________________________________________  
3. __________________________________________________________  

**Q8.** What additional defenses should a website use beyond hashing (e.g., salts, slow KDFs like bcrypt/Argon2, rate limiting)?  
Your answer: ________________________________________________________________

---

✅ **Submission:** Take a screenshot of your terminal showing `john --show` outputs for both MD5 and SHA-1 and submit with this worksheet.
