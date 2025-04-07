# Threat Detection with GuardDuty

**Author:** Felipe Gonzalez  
*
![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_sm42x3y4)


---

## Introducing Today's Project!

### Tools and concepts

GuardDuty
I learned how to use GuardDuty to detect suspicious activity, like someone stealing and using EC2 credentials or uploading malware.

IAM Roles
I saw how EC2 instances use IAM roles and how those roles can be misused if the credentials are leaked.

S3 Security
I learned how S3 bucket permissions work, and how GuardDuty helps detect unauthorized access or malware in buckets.

CloudTrail
I used CloudTrail to track what actions were taken with the stolen credentials and where they were used.

Malware Protection
I tested GuardDuty’s malware detection by uploading a safe test file (EICAR), and it correctly flagged it.

AWS CLI & Profiles
I practiced using the AWS CLI with different profiles to simulate an attacker using temporary credentials.

Security Best Practices
I understood how important it is to limit access, monitor activity, and use GuardDuty to catch threats quickly.

### Project reflection

I completed this project in approximately 2–3 hours, including setup, testing, and analyzing GuardDuty findings

Great project!

---

## Project Setup

Deployed a vulnerable web application environment using this CloudFormation template. Specifically, the stack includes:

EC2 Instance(s): Hosting the OWASP Juice Shop, a deliberately insecure web application for testing and learning.

IAM Role & Instance Profile: The EC2 instance is assigned a role with permissions, which are later exploited during the GuardDuty exercise.

S3 Bucket: Named something like vulnerablewebapp-thesecurebucket-<random>, this bucket contains a secret file used to simulate unauthorized access.

Security Groups & VPC Settings: Basic networking setup to allow traffic to the app and simulate real-world exposure.

Auto Scaling Group & Launch Template: For managing EC2 instances with specific configurations.

GuardDuty Detector (manually enabled later): To monitor and detect malicious activity.

This setup is intentionally insecure to help you test and explore AWS security monitoring and detection capabilities, especially GuardDuty.

The web app deployed is called Juice Shop. To strengthen my GuardDuty skills, I plan to simulate a variety of suspicious and potentially malicious activities such as port scanning, unauthorized access attempts, and IAM credential misuse targeting the application. I will then monitor how Amazon GuardDuty detects and classifies these activities, analyze the findings, and explore appropriate response actions. This hands-on approach will deepen my understanding of real-world threat detection using AWS-native security tools.

Amazon GuardDuty is a threat detection service that continuously monitors your AWS accounts, workloads, and data for malicious activity and unauthorized behavior.

It helps you:

🛡️ Detect threats like compromised instances, stolen credentials, or malware.

🔍 Monitor logs from AWS CloudTrail, VPC Flow Logs, and DNS queries.

🚨 Raise security findings with severity levels and detailed context.

🧠 Use machine learning, anomaly detection, and threat intelligence to spot unusual activity.

In short, GuardDuty gives you visibility into potential security issues in your AWS environment—without needing to deploy or manage infrastructure.

![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

The first attack I performed on the web app is SQL injection, which means I exploited a vulnerable input field like a login form to insert SQL code that the backend wasn’t expecting. 



Instead of entering a normal username and password, I typed in:

' OR 1=1

This clever little trick tells the database to ignore the actual login logic and just return true for everything. And just like that, I was in.

SQL injection is a security risk because it allows attackers to tamper with backend queries, often giving them unauthorized access to sensitive data, administrative functions, or even full control of the database. In some cases, attackers can read, modify, or delete data without ever needing a password.

It's like whispering secret instructions to the database through a crack in the wall. If the application doesn't validate inputs properly or use parameterized queries, it unknowingly obeys. This kind of attack is not only dangerous but also surprisingly easy to pull off on poorly secured applications, making it a top priority in both OWASP’s Top 10 and in real-world cloud threat detection with tools like GuardDuty.

![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Command injection is a critical web security vulnerability that occurs when an attacker is able to trick a server into executing unauthorized system commands—simply by entering malicious input where only plain data (like a username) was expected.

In this lab, we exploited the admin portal’s username field. Rather than entering a regular name, we injected system-level commands that the web server mistakenly executed. Imagine asking someone for their name, and instead they hand you instructions to unlock your phone and give it to them—that’s command injection in action.

The web app should have treated our input as harmless text, but due to a lack of input sanitization, it ran the embedded commands. This gave us access to something highly sensitive: the IAM credentials of the EC2 instance hosting the application.

To perform command injection, I targeted the Username field on the login page of the vulnerable web app (Juice Shop). Instead of entering a typical username, I inserted a specially crafted payload containing shell commands. The application, due to poor input validation, executed those commands directly on the underlying EC2 instance.

The injected command exploited the fact that the app was running system-level operations based on user input. Here’s a breakdown of the steps I used:

🔓 Step-by-Step Breakdown:
Accessed the login form of the Juice Shop admin portal.

Entered the following command in the username field:

$(TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600") && CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/ && CREDS=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN") && echo $CREDS | json_pp > frontend/dist/front

![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

The command injects this line into the server:

echo $CRED | json_pp > frontend/dist/frontend/assets/public/credentials.json

This writes the stolen IAM credentials to a file in the local file system of the Juice Shop app.
Note: qJuice Shop is most likely being served through CloudFront as a static web app. This means:
○ The frontend/dist/frontend directory is publicly served as part of the web app.
○ CloudFront acts as the CDN/front-door to the web content.
		
That’s why you can access this file at:

https://dlcjrnaymgmey.cloudfront.net/assets/public/credentials.json

This URL corresponds to:

/frontend/dist/frontend/assets/public/credentials.json

because that’s where the file was written during the injection.

🧭 In short:
The command injection didn’t include the CloudFront URL directly.

But it wrote the file to a location that is exposed via your app's CloudFront-distributed frontend.

CloudFront just acts as the public-facing entry point that makes that path available over HTT



![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

I'm using AWS CloudShell to simulate an external attacker who has stolen credentials and is now using them to access resources in a cloud environment they shouldn't belong to.

While I own both the victim EC2 instance and the CloudShell session, CloudShell actually runs under a different AWS account ID than the EC2 instance. This creates a realistic simulation of credential misuse across accounts—which is exactly what GuardDuty is designed to detect.

By configuring the AWS CLI in CloudShell with the stolen EC2 instance credentials, I'm creating a scenario where:

A separate AWS session (CloudShell) uses credentials meant for a different compute identity (the EC2 instance).

I then use those credentials to access resources like S3 buckets, mimicking what a real attacker would do after stealing IAM role credentials.


I ran the following commands to simulate how an attacker would locate, view, and extract sensitive credentials from a vulnerable web application:

1. wget
wget $JUICESHOPURL/assets/public/credentials.json
This command downloads the publicly exposed credentials.json file that was planted earlier through a command injection attack. It simulates how an attacker might exfiltrate stolen IAM credentials from a web-accessible location after compromising the server.

2. cat
cat credentials.json

I used cat to view the raw contents of the credentials file. This allowed me to confirm that the file contains valid AWS IAM temporary credentials, including:

AccessKeyId

SecretAccessKey

Token

Expiration

3. jq
cat credentials.json | jq
I piped the output to jq, a JSON parser, to pretty-print the credentials for easier inspection. This helps quickly extract or verify specific fields and simulates how an attacker might script the process of harvesting and using the credentials. 

The credentials I extracted from the vulnerable Juice Shop web app represent a temporary IAM role assigned to an EC2 instance.

By configuring them in a separate profile (stolen), I can isolate and test their permissions as if I were the attacker — using the credentials on a different machine (in this case, CloudShell).

This setup mimics real-world credential exfiltration, where an attacker uses stolen credentials from a public location to access AWS services elsewhere.

✅ Key benefits of using a new profile:
🔐 Safe testing without affecting your default AWS account access

🧪 Simulates external usage, triggering GuardDuty alerts like:

UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS

🔁 Enables easy switching between attacker (--profile stolen) and your admin account (default)


![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within approximately 6 minutes. Findings are timestamped with:

EventFirstSeen: 2025-04-03T20:46:22Z

CreatedAt (finding creation): 2025-04-03T20:52:51Z

This means GuardDuty detected and generated the alert 6 minutes after the unauthorized activity began, indicating relatively quick threat detection in the environment.

There were two instances of the same GuardDuty finding. 

Both findings were titled:

"Credentials for the EC2 instance role vulnerablewebapp-TheRole-6yiHfFZ30fjZ were used from a remote AWS account."

This indicates that the same compromised credentials were detected being used more than once from a remote AWS account, triggering two separate alerts with the finding type:

UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS

Both were flagged with a high severity of 8.0, showing that GuardDuty identified repeated suspicious use of the credentials.

"Credentials created exclusively for an EC2 instance using instance role vulnerablewebapp-TheRole-6yiHfFZ30fjZ have been used from a remote AWS account 792359258118."

This means the instance role credentials, which should have only been used by the EC2 instance, were exfiltrated and then used from outside the account — a clear sign of unauthorized access. The finding also identified the source IP, remote AWS account, and included the type:

UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS

This alerts the security team that the credentials were compromised and used inappropriately inside the AWS environment.

![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

I uploaded the EICAR test file to the S3 bucket to verify that Amazon GuardDuty Malware Protection is working as expected.

The EICAR test file is a harmless, industry-standard file developed by the European Institute for Computer Antivirus Research (EICAR). It’s designed to trigger antivirus and malware detection systems without containing any real threat.

Uploading this file allows us to safely test GuardDuty's ability to detect malware activity in an S3 bucket—confirming that Malware Protection is properly enabled and functional.

Yes, after uploading the EICAR test file to the S3 bucket, GuardDuty immediately detected it as malware. The finding details are as follows:

Finding Type: Object:S3/MaliciousFile

Severity: High

File Detected: EICAR-test-file.txt

Resource Affected: S3 bucket vulnerablewebapp-thesecurebucket-qu2vniy4nr6f

Detection Time: 2025-04-03 18:05:45 UTC

Finding ID: b8cafed82876c20def97cf253d07d932

File Hash (SHA-256): 275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f

This confirms that GuardDuty Malware Protection is working correctly and can effectively detect potentially harmful objects in your S3 buckets.

![Image](http://learn.nextwork.org/happy_cyan_mysterious_plantain/uploads/aws-security-guardduty_sm42x3y4)

---
