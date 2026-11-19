# Password Cracking with John the Ripper (JTR) & Networkwalks Tools

> **Note:** This is for educational purposes only.


## Project Overview

This report documents the practical security assessment focused on cryptographic hashing, password-protected file auditing, and dictionary-based password strength testing. The exercise evaluates how password-protected PDF documents store credential hashes and demonstrates how weak or common passwords can be recovered through offline dictionary attacks using both desktop-based and browser-based auditing utilities.


## Objectives

* Recover the passwords of protected PDF files using John the Ripper with Johnny GUI as well as the Networkwalks Hash Calculator and Password Cracker browser tools.
* Understand how password cracking works step by step from hash extraction to matching passwords.
* Recognize why using strong passwords is important for protecting locked files instead of relying on weak or common passwords.


## Purpose of the Lab

The purpose of this lab is to demonstrate why relying on short, predictable, or dictionary-based passwords leaves encrypted files vulnerable to offline compromise. Because encrypted documents store a cryptographic representation (hash) of the access password inside the file metadata, an attacker who obtains the file can extract the hash and test millions of candidate passwords offline without triggering account lockouts or network alerts. Auditing these mechanisms firsthand reinforces the operational necessity of high-entropy passphrases.


## Lab Architecture

* **Local Desktop Stack (Module 1):** Combines the command-line cryptographic engine John the Ripper (`1.9.0-jumbo-1`) with the Johnny GUI (`v2.2`) front-end to parse extracted `$pdf$...` hash text files and run local dictionary attacks.
* **Browser-Based Stack (Module 2):** Uses the Networkwalks Hash Calculator and Networkwalks Password Cracker in a web browser (requiring no installation) to extract the `$pdf$...` hash from a locked PDF and test passwords against a built-in dictionary list.


## Lab Configuration

| Component | Specification / Value |
| :--- | :--- |
| **Host Operating System** | Windows 11 PC (64-bit) |
| **CLI Cracking Engine** | John the Ripper `1.9.0-jumbo-1` (64-bit Windows binaries, `run/john.exe`) |
| **GUI Front-End** | Johnny GUI `v2.2` (`johnnyInstaller.exe`) |
| **Browser Auditing Suite** | Networkwalks Hash Calculator & Password Cracker (`networkwalks.com`) |
| **Target Artifacts** | `My Locked PDF1.pdf` (65.2 KB, Revision R4, Version V4, 128-bit encryption), `My Locked PDF 2`, `My Locked PDF 3` |
| **Extracted Hash Files** | `Hashmylockedpdf1`, `Hashmylockedpdf2`, `Hashmylockedpdf3` |
| **Attack Methodology** | Offline Dictionary Attack (Built-in 100-word list / JTR default wordlist) |


## Lab Procedure

### Phase 1: Local Desktop Environment (John the Ripper & Johnny GUI)

1. **Engine Deployment:** Downloaded the 64-bit Windows binaries for John the Ripper (`1.9.0-jumbo-1`) and extracted the archive to access the `run` directory containing `john.exe`.
2. **GUI Installation & Linking:** Installed Johnny GUI (`v2.2`), opened **Settings**, and mapped the **John the Ripper executable** path to `john.exe`, verifying active detection (`Detected John the Ripper 1.9.0-jumbo-1`).

   ![1_Johnny_GUI_Settings](1_Johnny_GUI_Settings.png)

3. **Hash Extraction & Formatting:** Extracted the cryptographic hash string from `My Locked PDF1` using a PDF Hash Extractor and saved it as a plain text file (`Hashmylockedpdf1`). If the string includes extra characters like `b'` at the very beginning, remove them so the hash starts strictly with `$pdf$`.

   ![2_My_Locked_PDF1_Hash](2_My_Locked_PDF1_Hash.png)

4. **Password Auditing & Verification (`My Locked PDF1`):** Opened Johnny, navigated to **Open password file > Open password file (PASSWD format)**, selected `Hashmylockedpdf1`, and executed the audit to reveal the password as `good-luck`, which opened `My Locked PDF1` to capture the flag: `nw{cybersecurity_flag_captured_2608}`.

   ![3_My_Locked_PDF1_Password](3_My_Locked_PDF1_Password.png)

   ![4_My_Locked_PDF1_Flag_Captured](4_My_Locked_PDF1_Flag_Captured.png)


### Phase 2: Browser-Based Environment (Networkwalks Tools)

1. **Hash Extraction:** Opened the Networkwalks Hash Calculator, uploaded `My Locked PDF1` (for NW Tools Lab WK3-PM2), and copied the extracted hash.

   ![5_NW_PDF_Hash](5_NW_PDF_Hash.png)

2. **Dictionary Execution:** Pasted the hash into the Networkwalks Password Cracker and clicked **Start Cracking**. `password1` opened the file to capture the flag: `nw{networkwalks_flag1_jtr_270521_1}`.

   ![6.1_NW_PDF_Password](6.1_NW_PDF_Password.png)

   ![6.2_NW_PDF_Flag_Captured](6.2_NW_PDF_Flag_Captured.png)


### Extra Verifications (My Locked PDF2 & My Locked PDF3)

1. **JTR:** Extracted the PDF hashes for `My Locked PDF2` and `My Locked PDF3`, saved them as text files (`Hashmylockedpdf2` and `Hashmylockedpdf3`), and opened each file in Johnny via **Open password file > Open password file (PASSWD format)** to reveal their respective passwords.
2. **Networkwalks Tools:** Repeated the exact same browser-based hash extraction and dictionary audit workflow for `My Locked PDF2` and `My Locked PDF3`, yielding the identical recovered passwords as the Johnny GUI workflow.

#### My Locked PDF2

* **JTR Password:**

  ![7.2_My_Locked_PDF2_Password](7.2_My_Locked_PDF2_Password.png)

* **Networkwalks Password:**

  ![7.4_My_Locked_PDF2_NW_Password](7.4_My_Locked_PDF2_NW_Password.png)

* **Flag Captured:**

  ![7.5_My_Locked_PDF2_Flag_Captured](7.5_My_Locked_PDF2_Flag_Captured.png)

#### My Locked PDF3

* **JTR Password:**

  ![8.2_My_Locked_PDF3_Password](8.2_My_Locked_PDF3_Password.png)

* **Networkwalks Password:**

  ![8.4_My_Locked_PDF3_NW_Password](8.4_My_Locked_PDF3_NW_Password.png)

* **Flag Captured:**

  ![8.5_My_Locked_PDF3_Flag_Captured](8.5_My_Locked_PDF3_Flag_Captured.png)


## Recommendations

Based on the successful recovery of the PDF passwords through offline dictionary attacks, the following defensive controls are recommended to protect sensitive documents:

* **Enforce High-Entropy Passphrases:** Avoid short, predictable, or common dictionary words (such as `password1`), which can be cracked within seconds using standard wordlists. Require passphrases of at least 12–16 characters combining uppercase letters, lowercase letters, numbers, and special symbols to exponentially increase offline cracking time.
* **Upgrade Document Encryption Standards:** The target PDF utilized 128-bit encryption (Revision R4, Version V4). Configure document creation tools to use modern 256-bit AES encryption (PDF Revision 6 / Acrobat X and later), which introduces stronger key-derivation iterations that significantly slow down hash-cracking engines.
* **Control File Distribution & Access:** Because an attacker can extract the encryption hash (`$pdf$`) and run unlimited offline password guesses without triggering lockout alerts, store sensitive files in encrypted, access-controlled repositories.  


## Tools & Resources

* **John the Ripper (JTR Jumbo 1.9.0-1):** Open-source command-line password security auditing and hash recovery engine (`https://www.openwall.com/john/`).
* **Johnny GUI (v2.2):** Cross-platform graphical front-end for managing John the Ripper sessions, hash files, and attack configurations (`https://openwall.info/wiki/john/johnny`).
* **PDF Hash Extractor:** Online hash extraction interface used for protected PDF files (`https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php`).
* **Networkwalks Hash Calculator:** Client-side browser utility used for extracting hashes (`https://networkwalks.com/hash-calculator/`).
* **Networkwalks Password Cracker:** Browser-based dictionary attack lab tool for testing extracted PDF hashes against wordlists (`https://networkwalks.com/password-cracker/`).
* **Adobe Acrobat Reader DC:** Used to validate the recovered passwords, open the locked PDFs, and capture the flags.


## Author
Olakunle Odesanya  
Github: [Lakunsin](https://github.com/Lakunsin)  
Aspiring SOC Analyst

LinkedIn: https://www.linkedin.com/in/olakunle-o-04a51a257

## Project Information
Program Name: Cybersecurity Internship at Networkwalks (Batch B084) | Week: 03 | Project: Password Cracking with JTR & Networkwalks Tools | Repository: GitHub
