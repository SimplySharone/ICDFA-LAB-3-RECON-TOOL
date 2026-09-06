# Lab-3-Assignment-
LAB 3 – BASH SCRIPTING: BUILD A RECONNAISSANCE TOOL EVIDENCE REPORT
# ICDFA | WADF - 2026-M02 | Cybersecurity Practical Lab

# LAB 3 – BASH SCRIPTING: BUILD A RECONNAISSANCE TOOL

**Submission Date:** 6th SEPTEMBER, 2026

---

## Student Identification Details

| **Student Identification Details** | **Information**                     |
| ---------------------------------- | ----------------------------------- |
| **Full Name**                      | OMOWUMI SHARON                      |
| **Registration Number**            | FDFC2617050                         |
| **Linux Distribution / Version**   | KALI DEBIAN LINUX                   |
| **VM Hostname**                    | Sharonkali-lab                      |
| **Lab Target**                     | Metasploitable 2 – `192.168.56.102` |

---


## 1. Project Overview & Authorisation Boundary
This repository contains a modular Bash automation script developed to streamline initial information-gathering phases during authorized security assessments. 

---

## 2. Lab Execution & Evidence

### Phase 1: Environment Setup, Verification, & File Assembly
The following consolidated command sequence initializes the workspace, confirms core dependencies are available, creates the script asset, and establishes execution permissions.

```bash
mkdir -p ~/lab3-recon && cd ~/lab3-recon && pwd
command -v bash nmap whatweb dirb
touch recon_tool.sh && chmod +x recon_tool.sh && ls -l recon_tool.sh
```

#### 📸 Screenshot 1: Workspace Initialization & Permissions Verification

<img width="1920" height="1003" alt="Screenshot 1 lab 3" src="https://github.com/user-attachments/assets/a79cda34-2ae1-40a0-8e35-87da1d24aa11" />


*(Screenshot Guidelines: Ensure your image captures the directory path ending in `/lab3-recon`, the path listings for the verification commands, and the `ls -l` permission line showing the green filename with executable `-rwxr-xr-x` privileges.)*

---

### Phase 2: Core Script Implementation
Below is the full source code for the automated scanner. It utilizes input sanitation checks, explicit function definitions, and a structured processing menu.

```bash
#!/usr/bin/env bash

echo "ICDFA Beginner Reconnaissance Tool"
echo "---------------------------------"
echo "Use only against authorised lab targets."
echo

read -rp "Enter authorised target IP address or domain: " target

if [[ -z "\$target" ]]; then
    echo "Error: no target was entered."
    exit 1
fi

if [[ "\$target" =~ [[:space:]] ]]; then
    echo "Error: target must not contain spaces."
    exit 1
fi

check_tool() {
    if ! command -v "\$1" >/dev/null 2>&1; then
        echo "Error: required tool '\$1' is not installed or not in PATH."
        exit 1
    fi
}

echo "Select a reconnaissance tool:"
echo "1) WhatWeb"
echo "2) Nmap"
echo "3) DIRB"
echo "4) Exit"
read -rp "Enter your choice [1-4]: " choice

case "\$choice" in
    1)
        check_tool whatweb
        echo "[+] Running WhatWeb against \$target"
        whatweb "http://\$target"
        ;;
    2)
        check_tool nmap
        echo "[+] Running Nmap service detection against \$target"
        nmap -sV "\$target"
        ;;
    3)
        check_tool dirb
        echo "[+] Running DIRB against \$target"
        dirb "http://\$target"
        ;;
    4)
        echo "Exiting. No scan was run."
        exit 0
        ;;
    *)
        echo "Error: invalid menu choice."
        exit 1
        ;;
esac
```

#### 📸 Screenshot 2: Complete Script Integrity
```bash
cat recon_tool.sh
```
<img width="1920" height="1003" alt="Screenshot 2 lab 3" src="https://github.com/user-attachments/assets/63822898-4dc4-4cc3-b743-70076769ae2e" />


*(Screenshot Guidelines: Capture the output of the `cat` operation demonstrating that the entire script has been written accurately into the local file.)*

---

### Phase 3: Tool Execution & Verification
The tool was successfully tested against the authorized lab target: `192.168.56.102` (Metasploitable2 target profile).

#### Test 1: Web Fingerprinting (WhatWeb)
```bash
./recon_tool.sh
# Select Option 1
```
#### 📸 Screenshot 3: Interactive Menu & WhatWeb Execution Output

<img width="1920" height="1003" alt="Screenshot 3 lab 3" src="https://github.com/user-attachments/assets/fe8e4de2-0309-48d6-b13f-e65a71c8c48b" />


*(Screenshot Guidelines: Crop the terminal window showing the input target `192.168.56.102`, menu option selection `1`, and the resulting line identifying Apache 2.2.8, PHP 5.2.4, and the Metasploitable2 title signature.)*

---

#### Test 2: Port & Service Verification (Nmap)
```bash
./recon_tool.sh
# Select Option 2
```
#### 📸 Screenshot 4: Nmap Service Detection Execution Output

<img width="1920" height="1003" alt="Screenshot 4 lab 3" src="https://github.com/user-attachments/assets/653ff623-de6a-436d-9fe9-5fa98901e030" />


*(Screenshot Guidelines: Capture the complete service version chart detailing the open network sockets including FTP vsftpd 2.3.4, SSH OpenSSH 4.7p1, and HTTP Apache httpd 2.2.8.)*

---

## 3. Tool Mechanism Explanations

| Evaluated Concept | Function & Operational Context |
| :--- | :--- |
| **Purpose of the Script** | Consolidates multiple disjointed command-line utilities into a unified, interactive prompt framework to safely standardise operational workflows. |
| **Variable Storage (`read`)**| Suspends active runtime execution to capture user keystrokes from standard input, mapping the string data straight to memory names. |
| **Dynamic Tool Swapping** | Employs conditional control branches to quickly isolate and match the chosen configuration code block without running unneeded modules. |
| **Omitting Hardcoded Targets**| Decouples system infrastructure information from raw text source code, keeping the application scalable and safe for public storage. |
| **Permission Elevations** | Signals kernel handlers to evaluate the text file container as a runnable binary sequence rather than flat read-only configuration text. |

---

## 4. Understanding Reference Guide

#### 1. What does the shebang do?
It tells the operating system loader exactly which interpreter path (`/usr/bin/env bash`) must be used to read and execute the trailing instructions found in the script file.

#### 2. What does `read -rp` do?
The `-r` flag ensures that raw characters like backslashes are read literally instead of being processed as escape mechanisms, while `-p` outputs a descriptive input label text inline before waiting for user typing.

#### 3. Why quote `"$target"`?
Enclosing strings within double quotes blocks the terminal from splitting data components apart or expanding whitespace into multiple individual command arguments.

#### 4. What does `-z` test?
It evaluates a target string variable to verify if it has a data string length of exactly zero, acting as an empty-field catch-all.

#### 5. What is the purpose of `exit 1`?
It halts the runtime sequence immediately and communicates an explicit failure state return signal back to the parent shell.

#### 6. Why is `case` suitable for menus?
It provides an organized match interface that tracks individual text items against distinct target options, avoiding deep, difficult-to-read `if/else` logic structures.

#### 7. What does `;;` mean?
It functions as a strict control terminator, mapping the absolute boundary of a matched pattern group inside an active `case` condition block.

#### 8. What does `*` mean in the case statement?
It acts as a universal wildcard match that catches any text input string that failed to map cleanly onto prior valid patterns.

#### 9. Why does WhatWeb/DIRB use `http://$target`?
Since these tools evaluate protocol layers, they require a fully defined application-layer scheme prefix to properly structure their web resource calls.

#### 10. What does `nmap -sV` do?
It initiates banner-grabbing connections and interrogation probes against available port listeners to catalog application service software and exact engine versions.

#### 11. What does `command -v` check?
It look up executable binary files within local path environments to confirm whether a utility is available for use without actually starting it.

#### 12. What does `chmod +x` do?
It adds executable permissions to a target file container's system attribute byte flags.

#### 13. What does `./` mean?
It instructs the operational environment to restrict its program lookup window strictly within the present active working directory.

#### 14. What does `tee` do?
It duplicates a data pipeline stream, pushing output content out to the visible interface console while copying it into a specified log file.

#### 15. Which requirement prevents hardcoding?
The explicit lab requirement to **"Prompt for a target IP/domain"** dynamically through variables rather than hardcoding static configuration data.
