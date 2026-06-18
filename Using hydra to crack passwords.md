link to the room:

https://labex.io/labs/linux-using-hydra-to-crack-passwords-415960?course=cybersecurity-labs-for-beginners

Introduction
In this lab, you will learn about password vulnerabilities and how to use Hydra, a popular security tool for testing password strength. You'll simulate a brute-force attack on a practice website to understand how weak passwords can be easily compromised.

This hands-on experience will demonstrate why strong passwords are crucial in cybersecurity. You'll gain practical skills in configuring Hydra, analyzing attack results, and evaluating password security measures.

---

Exploring the Target Website
In this step, we'll examine the website we'll be testing and introduce the concept of brute-force attacks. Before we start using automated tools, it's important to understand how basic authentication systems work and what we're trying to accomplish.

Open the Web 8080 tab in your lab environment. This will display a web browser interface where you'll see a simple login page with fields for entering a username and password. This is similar to login pages you encounter on many websites every day.

<img width="1200" height="862" alt="image" src="https://github.com/user-attachments/assets/a3dfae78-5dd3-48af-8c24-763d659ef59f" />


Let's try some basic login attempts to understand how the system responds:

First attempt:

Username: test
Password: password123
Second attempt:

Username: admin
Password: admin
After each attempt, you'll receive an "Invalid username or password" message. This generic response is intentional - it doesn't tell you whether the username exists or if just the password was wrong. This is called "security through obscurity" and helps prevent attackers from gathering information about valid accounts.

<img width="1120" height="763" alt="image" src="https://github.com/user-attachments/assets/47e6dc1c-e734-4cb3-9f47-cdd95cd121fd" />

Notice that you can keep trying different combinations without any restrictions. In production systems, security measures like account lockouts or CAPTCHAs would typically be implemented after several failed attempts to prevent automated attacks. The absence of these protections in our lab environment makes it suitable for demonstrating brute-force techniques.

Let's try some basic login attempts to understand how the system responds:

First attempt:

Username: test
Password: password123
Second attempt:

Username: admin
Password: admin
After each attempt, you'll receive an "Invalid username or password" message. This generic response is intentional - it doesn't tell you whether the username exists or if just the password was wrong. This is called "security through obscurity" and helps prevent attackers from gathering information about valid accounts.

Notice that you can keep trying different combinations without any restrictions. In production systems, security measures like account lockouts or CAPTCHAs would typically be implemented after several failed attempts to prevent automated attacks. The absence of these protections in our lab environment makes it suitable for demonstrating brute-force techniques.

What you've just done manually - trying different username and password combinations - is essentially a manual version of a brute-force attack. In real attacks, hackers use automated tools to try thousands of combinations per second. This exercise shows why simple passwords like "password123" or "admin" are extremely vulnerable, and why systems need proper security measures to prevent such attacks.
What you've just done manually - trying different username and password combinations - is essentially a manual version of a brute-force attack. In real attacks, hackers use automated tools to try thousands of combinations per second. This exercise shows why simple passwords like "password123" or "admin" are extremely vulnerable, and why systems need proper security measures to prevent such attacks.

Exploring the Target Website
In this step, we'll examine the website we'll be testing and introduce the concept of brute-force attacks. Before we start using automated tools, it's important to understand how basic authentication systems work and what we're trying to accomplish.

In real-world scenarios, attackers often use extensive lists of passwords from past data breaches. These lists contain millions of passwords that were exposed in security incidents. For our lab, we'll use a smaller list of common weak passwords found on the internet to demonstrate the concept. This list has already been prepared for you and is located at /home/labex/project/500-worst-passwords.txt.

head -n 10 500-worst-passwords.txt

<img width="1192" height="540" alt="image" src="https://github.com/user-attachments/assets/c5631846-d34f-474c-b549-2239f4ca23aa" />

These passwords are common choices that many people unfortunately still use. Notice how simple and predictable they are - mostly numeric sequences or common words. This is precisely why attackers often try these first in their attempts to gain unauthorized access.

By using a list like this, an attacker can greatly speed up their brute-force attack. Instead of trying every possible combination of characters (which could take years), they start with the most likely passwords, significantly increasing their chances of quick success. In cybersecurity, we call this a "dictionary attack" - a smarter version of brute-forcing that uses known passwords rather than random guesses.

Setting Up Hydra
Now that we have our list of passwords, let's set up Hydra, the tool we'll use to automate our brute-force attack simulation. Hydra is a powerful password-cracking tool that helps security professionals test the strength of passwords by systematically trying different combinations.

First, we'll create a simple text file containing common usernames that we want to test. This is important because many systems use predictable default usernames that attackers might try first. Run these commands:
Open the Web 8080 tab in your lab environment. This will display a web browser interface where you'll see a simple login page with fields for entering a username and password. This is similar to login pages you encounter on many websites every day.


cd ~/project
echo -e "admin\nuser\nroot" > ~/project/usernames.txt
cat ~/project/usernames.txt

<img width="1060" height="257" alt="image" src="https://github.com/user-attachments/assets/ea63d8c1-4dbe-48cd-ab74-83198b67544a" />
The first command (cd ~/project) ensures we're in the correct working directory. The second command creates a file called usernames.txt containing three common usernames (admin, user, and root), each on a new line. The third command displays the contents of the file so we can verify it was created correctly.

Now, let's make sure Hydra is installed. In a real-world scenario, you would typically install it yourself, but in this lab environment

Hydra works by taking lists of potential usernames and passwords, then attempting to authenticate with each combination against a target service. It supports many protocols including HTTP, FTP, SSH, and more. The tool is particularly useful for penetration testers to identify weak passwords in a system.

To verify Hydra is working correctly, we can check its version and available options with:

hydra -h
<img width="843" height="870" alt="image" src="https://github.com/user-attachments/assets/5b3fada0-7732-4d50-8bf2-9d3b89b1d380" />

Using Hydra for Password Cracking
Now that we have Hydra installed and our password list ready, let's use it to simulate a brute-force attack on our practice website. Before we begin, it's important to understand that Hydra is an automated tool that systematically tries different username and password combinations against a login page - this is called a brute-force attack.


Run the following Hydra command:
hydra -L ~/project/usernames.txt -P ~/project/500-worst-passwords.txt localhost -s 8080 http-post-form "/:username=^USER^&password=^PASS^:Invalid username or password" -o ~/project/hydra_results.txt


Let's break down this command piece by piece to understand what each part does:

hydra: This is the command to start the Hydra tool.
-L ~/project/usernames.txt: Specifies the file containing potential usernames to try.
-P ~/project/500-worst-passwords.txt: Points to the file with common passwords we'll test against each username.
localhost -s 8080: The target web server address (localhost) and port (8080) we're testing.
"/:username=^USER^&password=^PASS^:Invalid username or password": This tells Hydra:
The login page URL (/)
How the login form fields are named (username and password)
What the error message looks like when login fails
-o ~/project/hydra_results.txt: Saves all successful attempts to this output file.


<img width="1212" height="287" alt="image" src="https://github.com/user-attachments/assets/22ea3913-b206-45d3-bf50-1fdfbd60e163" />

verify
cat ~/project/hydra_results.txt
<img width="1213" height="151" alt="image" src="https://github.com/user-attachments/assets/59f411bd-9802-4e30-856c-cbc214ddeab3" />

<img width="687" height="543" alt="image" src="https://github.com/user-attachments/assets/168765d3-702c-4696-9b81-3070657c2820" />






Summary
In this lab, you have learned the fundamentals of password security through hands-on experience with Hydra, a powerful password-cracking tool. You've explored how brute-force attacks work and why they can compromise weak passwords, while also understanding the critical role of strong password practices in cybersecurity.

This exercise has demonstrated why cybersecurity professionals emphasize using complex, unique passwords. Remember to apply these skills ethically as you continue exploring security tools and techniques in your learning journey.
