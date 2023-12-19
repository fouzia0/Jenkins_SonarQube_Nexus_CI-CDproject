# Jenkins_SonarQube_Nexus_CI-project

We're setting up a Continuous Integration/Continuous Deployment (CI/CD) project.
The configuration involves Jenkins, Nexus, and SonarQube servers.
Specific prerequisites are required for each server.
The infrastructure includes a Jenkins server (t2 small - Ubuntu).
Additionally, there's a Nexus server (t2 medium - Ubuntu)
The SonarQube server is part of the setup, configured with CentOS 7 (t2 large).
For Jenkins, certain plugins are needed, including Nexus Artifact Uploader.
Other required Jenkins plugins are SonarQube Scanner, Pipeline Maven Integration, Pipeline Utility Steps, and Build Timestamp.
Bootstrap scripts for both Nexus and SonarQube servers are attached to the repository.

# Launch Instance:

# step-1: Launch Jenkins-server: (AMI-Ubuntu 20.04 LTS,t2-small,), security group (ssh-port22, and TCP custome-port8080)
Initiate the launch of a new instance.

# Step 2: Go to JENKINS GUI:
(a) Navigate to the Jenkins GUI.

(b) Inside the Jenkins GUI:

Go to "Manage Jenkins."
Click on "Tools."

(c) JDK Installation:
Add a new JDK by selecting "Add JDK."
Specify the following details:
JDK Name: OracleJDK8
JAVA_HOME: /usr/lib/jvm/java-8-openjdk-amd64

(c) Maven Installations:
Add a new Maven installation by selecting "Add Maven."
Provide the following details:
Name: MAVEN3
Install from Apache
Version: 3.9.6
Click "Apply & Save"

# Step 3: Nexus Server Setup:
(a) Initiate the launch of the Nexus server:
Select AMI: CentOS, instance type: t2-Large.
Configure security groups to allow OpenSSH (22) from anywhere and TCP (8081) from anywhere.
In "Advance Details," include a Bash script for additional setup.

Complete the launch process.

(b) Perform an SSH login and verify the status using below command : sudo systemctl status nexus
Ensure the status is active.

(c) Access the Nexus server using the public IP address on port 8081:
Open a web browser and go to PublicIpAddress:8081.
Log in to the web UI, configure plugins, and set a personalized password.


# Step 4: SonarQube Server Setup:

(a) Launch an EC2 instance with Ubuntu 20.04 LTS SSD:
Choose a t2-medium instance type.
Configure security groups to allow SSH (22) from your IP, HTTP (80) from anywhere, and port 9000 from anywhere.

Add user data from the GitHub repository.

(b) Perform an SSH login and verify the status using the command :  sudo systemctl status sonarqube
Ensure the status is active.

(c) Access the SonarQube server using the public IP address and port number:

Open a web browser and go to PublicIpAddress:PortNumber.
Log in to the web UI, configure plugins, and set a personalized password.

# SonarQube-Jenkins Integration:
# Step-5: Jenkins Integration with SonarQube:

(a) Navigate to the Jenkins Dashboard:

Go to "Manage Jenkins."
Click on "Tools."
Select "SonarQube Scanner Installation."
Add a new SonarQube Scanner named "sonar4.7":
Install automatically.
Version: SonarQube Scanner 4.7.0.2747.
Click "Apply & Save."

(b) Generate a token:
In SonarQube GUI, go to admin > your account > security > generate token.
Provide a name (e.g., "jenkins") and generate the token.
Copy the generated token.

(c) Configure SonarQube server in Jenkins:
Go to Jenkins Dashboard > Manage Jenkins > System > SonarQube servers.
Click on "Environment variables":
Name: sonar
Server URL: http://PrivateIPAddress:9000
Add Jenkins credentials:
Kind: secret text
Paste the generated token.
Token ID: MySonarToken
Description: MySonarToken
Click "Add & Save."

(d) Configure Build Timestamp:
Go to Jenkins Dashboard > Manage Jenkins > Configure System.
In Build Timestamp, remove yy, ss, and z from the pattern.
Click "Apply & Save."

# SonarQube-Jenkins Setup Credentials:

# Step 6: First Crosscheck Jenkins Credentials: If SonarQube Credentials Present, Skip Step 6:

(a) Navigate to the Jenkins Dashboard:
Go to "Manage Jenkins."
Click on "Credentials."
Select "System."
Choose "Global credentials (unrestricted)."

(b) Add new credentials if not present:
Click on "New Credentials."
Enter the following details:
Username: admin
Password: admin
ID: sonarqubelogin
Description: sonarqube login
Click "Create" to add the credentials.
(Note: If SonarQube credentials are already present, you can skip this step.)

# SonarQube GUI Setup:

# Step 7: Go to SonarQube GUI:

(a) Navigate to Quality Gate:
Go to Quality Gate.
Create a new Quality Gate with the following details:
Name: vprofile-QG
Save and add conditions:
On overall code
Bugs: 100
Save.

(b) Create another Quality Gate:
Go to Quality Gate.
Create a new Quality Gate with the following details:
Name: vprofile-QG

(c) Configure SonarQube Projects:
Go to SonarQube Projects.
Select the project.
Navigate to Project Settings > Quality Gate.
Choose the quality gate: vprofile-QG.
In Project Settings, select Webhook:
Create a new webhook named "jenkin-ci-webhook."
Jenkins URL: http://jenkin-public-ip/sonarqube-webhook
Create.

# Nexus GUI Setup:

# Step 8: Go to Nexus GUI:

(a) Create Repository:
Log in to the Nexus server.
Navigate to Repository > Repository.
In Settings, select "Create Repository."
Choose "maven2 hosted."
Provide the following details:
Name: vprofile-repo
Click on "Create Repository."

# Nexus-Jenkins Setup Credentials:

# Step 9: Nexus Setup Credentials with Jenkins:

(a) Navigate to the Jenkins Dashboard:
Go to "Manage Jenkins."
Click on "Credentials."
Select "System."
Choose "Global credentials (unrestricted)."

(b) Add new credentials:
Click on "New Credentials."
Enter the following details:
Username: admin
Password: admin
ID: nexuslogin
Description: nexuslogin
Click "Create" to add the credentials.

# Creating the Continuous Integration (CI) Pipeline through Jenkins:

# Step 10: Creating the CI-CD Job:

(a) Follow these steps:
Click on "+New Item."
Enter Item Name: CI-CD_Project.
Choose Pipeline script.
Copy and paste the provided script.
Update the IP to the private IP of the Nexus server.
Click "Apply & Save."

(b) Run the CI-CD job:
Execute the CI-CD job and monitor the entire process by checking the progress in the lower-left corner.

# Step 11: Verification After Successful CI - Project Run:

(a) SonarQube GUI Verification:
Go to the SonarQube GUI server's 'Quality Gate' page.
Refresh the page and observe the results.

(b) Nexus GUI Verification:
Check the Nexus GUI to verify the created artifact in the repository.
<img width="960" alt="jenkis gui" src="https://github.com/fouzia0/Jenkins_SonarQube_Nexus_CI-project/assets/146019530/a02ec88e-c2c3-4a69-a3ca-48672a9b6bab">

<img width="494" alt="BBB" src="https://github.com/fouzia0/Jenkins_SonarQube_Nexus_CI-project/assets/146019530/514de9b7-fbb8-4fc8-b807-ef7c9442d18c">

<img width="655" alt="AAA" src="https://github.com/fouzia0/Jenkins_SonarQube_Nexus_CI-project/assets/146019530/91903c2c-ad12-4af6-9648-28e0203df8cf">


