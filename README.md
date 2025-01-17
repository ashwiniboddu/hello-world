# DevOps Project - Ashwini

## Deployment of Hello-World Application

This project demonstrates deploying a Hello-World application using Jenkins, Docker, and Tomcat Image .

---

## Repository URL


---

## Prerequisites
1. **Launch an Instance:**
   - **OS:** Ubuntu 22.04
   - **Instance Type:** t2.medium
   - **Storage:** 30 GB

2. **Connect to the Instance:**
   # switch to root-user
    sudo su

Step 1: Update Packages and Install Dependencies
1. Update default packages:
apt update -y
2. Install Java 17:
sudo apt install fontconfig openjdk-17-jre

Step 2: Install and Configure Jenkins
 1. Install Jenkins : 
 sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
2. Access Jenkins:
---> Jenkins runs on port 8080 by default.
---> Open the Security Group and edit inbound rules to allow traffic on port 8080.
---> Access Jenkins at http://<public-ip>:8080.
3. Retrieve the default admin password:
cat /var/lib/jenkins/secrets/initialAdminPassword
4. Complete the Jenkins setup:
Change the default password.
Install suggested plugins.

Step 3: Install and Configure Docker
1. Install Docker:
apt install docker.io
2. Give permissions to run Docker:
sudo chmod 666 /var/run/docker.sock
3. Install necessary Jenkins plugins:
Go to Manage Jenkins -> Plugins -> Available Plugins.
Install:
---> Pipeline: Stage View
---> Docker Pipeline
---> Deploy to Container
4. Configure Maven:
---> Go to Manage Jenkins -> Tools -> Add Maven.
---> Name: maven s/w.
---> Enable automatic installation.
5. Add DockerHub credentials in Jenkins:
---> Generate a personal access token from DockerHub.
---> In Jenkins:
Navigate to Manage Jenkins -> Credentials -> Global -> Add Credentials.
---> Use:
Kind: Username and Password
Username: DockerHub username.
Password: Personal access token.
ID: dockerhub-cred.

Step 4: Create and Run the Jenkins Pipeline
1. Create a new pipeline in Jenkins:
---> Go to Dashboard -> New Item -> Pipeline.
---> Name the project Hello-World.
2. Configure the pipeline script to build and deploy the application.
3. Run the pipeline:
---> Trigger the Jenkins pipeline to build and deploy the application.
---> The Docker container will expose your application on port 8081.


Step 5: Configure Tomcat Server
Once the container is running, modify the required Tomcat configuration files directly in the container.
1. Access the Running Container:
docker exec -it hello-world-tomcat bash
2. Modify Configuration Files:
---> Change server.xml to update the port:
Path: /usr/local/tomcat/conf/server.xml.
---> Update <Connector port="8080" to <Connector port="8081".
3. Grant admin permissions:
---> Edit context.xml
vi /path/to/tomcat/webapps/manager/META-INF/context.xml
---> Edit the allow tag -----> ".*" />
4. Configure users:
---> Edit tomcat-users.xml located in the conf folder
<role rolename="manager-gui" />
<user username="tomcat" password="tomcat" roles="manager-gui" />
<role rolename="admin-gui" />  
<user username="admin" password="admin" roles="manager-gui,admin-gui"/>
5. Restart the Tomcat Container:
docker restart hello-world-tomcat
6. Access the Application:
---> URL: <public-ip>:8081

the final outcome :
https://imagekit.io/tools/asset-public-link?detail=%7B%22name%22%3A%22Hello-World.png%22%2C%22type%22%3A%22image%2Fpng%22%2C%22signedurl_expire%22%3A%222028-01-16T15%3A03%3A56.766Z%22%2C%22signedUrl%22%3A%22https%3A%2F%2Fmedia-hosting.imagekit.io%2F%2Fa78e50b2c9394f06%2FHello-World.png%3FExpires%3D1831647837%26Key-Pair-Id%3DK2ZIVPTIP2VGHC%26Signature%3DaOFNQVcVT-tUqfAY8Mt-yDWQSbkVWaR~gSQpgNOpvhgMfn5sgKt-d17OHIcsLjaverGnbz5ACnwX8JiggeGni6kFmJkZP1vFQpLlcHU5aMl0yFPtdveIuGFDTp8VQDUt~fCwTgJZAgDvHhDr01C2pAn~QtyjTjIsIcFx2Vwlhkeg28xN9W1Djb3act9nOf7qf7mKhIBatr7EOstAuyrDF4UXyf4erSIjpac5tjvWA6GexoX3u2b~XOo3jfIKTwUieW8lAkvHZRFIjMHy89MQNjhkj3hQe4r6TYWgpmlUe51iL5MIUWu48L5FfRl8X0NrtZxHHpEnxyAEaDizzhpZqg__%22%7D

Note: 
Ensure all changes to configuration files are saved before restarting services.
Troubleshooting : 
Port Conflict: Ensure Tomcat and Jenkins are running on separate ports.
