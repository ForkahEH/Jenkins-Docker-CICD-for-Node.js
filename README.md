# Jenkins-Docker-CICD-for-Node.js

# Overview

# Step 1: Provision EC2 Instance for Jenkins

1. In the AWS Console Home, search for and select "EC2" resource.

2. In the EC2 Console, select "Launch Instances".

3. In the "Launch Instance" page, enter the name of the webserver: Jenkins Web server.

   Instance type: t2.medium
   
   Key pair: CICD

   Select Edit in Network Settings
   
   select create security group

   Security group name: Jenkins SG

   Rules: SSH, port 22 and Custom TCP, port 8080

   SSH source: My IP, Custm TCP source: My IP

   NB: The source, "My IP" is selected for added security.
   
5. Click "Launch Instance"

6. Click "View Instances"
![Screenshot 2023-09-29 112745](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/901f1324-dad3-444e-a32c-7135ddc387d9)

7. Refresh until status check for the instance shows "2/2 checks passed".

8. In this project, we'll connect to the instance using SSH.
![Screenshot 2023-09-29 113635](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/a7b1ba14-3f6b-442e-b8b7-49dbdf985e9d)

# Step 2: Install Jenkins on EC2 Instance

1. Follow the Jenkins installation instructions from the Jenkins Documentation to install Jenkins: https://www.jenkins.io/doc/book/installing/linux/

   Java is installed

    ![Screenshot 2023-09-29 114800](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/aba6d392-8775-42b6-b3a7-6978e8704473)

   Jenkins is installed

    ![Screenshot 2023-09-29 115155](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/bbf2125c-b106-4a7c-91fa-8fe3f1f0d3c2)

3. The Jenkins webserver is accessed from the browser using the public ip and the Jenkins port number 8080

   ![Screenshot 2023-09-29 115309](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/6bb90fe6-8d63-422d-a59a-8337b016b478)

5. The Jenkins server password is retrieved using the command: sudo cat /var/lib/jenkins/secrets/initialAdminPassword

![Screenshot 2023-09-29 115539](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/6fe46a2d-f125-473b-9a2d-e2aa88fd4a24)

5. Login to the Jenkins server

![Screenshot 2023-09-29 115818](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/12a39fd8-aec4-474e-aa24-1bd785188111)

6. Enter the relevant details and voila!! Jenkins is ready.

![Screenshot 2023-09-29 120029](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/068aa685-5950-4606-bdac-64e4e81507c9)

   NB: An SSH key is created to connect the Jenkins server and the Github repo and a webhook created.

   Run the following commands on the terminal: ssh-keygen

   sudo cat id_rsa.pub

   Copy the public key. Go to Github > Settings > SSH and GPG keys > New SSH key.

   Paste the public key and save.

   ![Screenshot 2023-09-29 144810](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/ce4e47b6-f94e-49ad-bef4-bbca5a107145)

   Configure a webhook to allow automatic build of the project. This will fail because of the inbound rules of the Jenkins SG allowing only traffic from "My IP" via SSH and 
   port 8080.

   I had to allow all SSH and port 8080 traffic to configure the webhook.

   ![Screenshot 2023-09-29 164022](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/498a6287-4106-4b3c-b335-5f83515fde3c)


8. Select "New Item", enter the name of the project, select Freestyle and press OK
  
9. Enter the configuration details

   Description: CICD pipeline using Jenkins for node.js application

   Project url : https://github.com/ForkahEH/node-todo-cicd.git

11. In Source Core Management, select add Jenkins credentials.
    
    Enter the Github username where the project is stored.

    ![Screenshot 2023-09-29 132650](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/77e4e0c1-6c70-4a72-83c0-bd95be68e0b9)

    
    In the password section, a github token is entered.
    
    To obtain the Github token, open the github account and select settings > Developer settings . Tokens(classic) > Generate new 
    token > Generate new personal access token(classic).

    Enter the relevant details and select "Generate new token".
    
    Copy the token and paste it in the password section.

    Select "Add".

12. Select "Build Now"....and the repo is cloned from Github.

![Screenshot 2023-09-29 151318](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/56e382fb-5d7b-4f63-9e5b-db03fb5aa5ea)


12. Confirm the repo has been cloned on the EC2 instance.

    ls /var/lib/jenkins/workspace/Jenkins-Docker-CICD-for-Node.js

     ![Screenshot 2023-09-29 123356](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/0fc3f1d3-cdb6-4d93-b620-ae2e36acbec5)

14. Check the README file and install the necessary dependencies on the EC2 instance.

![Screenshot 2023-09-29 124323](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/30e77be5-4fd7-49d1-b17b-9d0ddecfc66b)

13. The app is running on port 8000.

     In order to access the app on the browser, the Jenkins SG security group settings inbound rules must be edited to allow traffic from port 8000.

     In this case, the source is "Everywhere" since the app would be accessed by people over the internet.

    ![Screenshot 2023-09-29 124819](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/b2b9dba9-cd86-4cbb-ac74-69c7931cde84)


15. The app is accessed from the browser using the public ip and the port number 8000.
![Screenshot 2023-09-29 125342](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/5d02de4c-6a7f-427b-9447-23708794bf57)

# Step 3: Dockerize the application

1. On the EC2 instance, install Docker.
   Follow the Docker installation instructions from the Docker Documentation to install Docker: :https://docs.docker.com/engine/install/ubuntu/
![Screenshot 2023-09-29 142131](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/3313ed9a-215c-499b-a446-3179e044a41e)

2. cd /var/lib/jenkins/workspace/Jenkins-Docker-CICD-for-Node.js
![Screenshot 2023-09-29 142407](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/648c39f7-406f-4189-a67c-5d301e9ffe6e)

3. Build the docker image of the app: docker build . -t todoapp
![Screenshot 2023-09-29 142540](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/1a7adc0a-1cf9-4545-abb0-50d9cb1d7051)

4. Run the Docker container:  docker run -d --name todo -p 8000:8000 5fcf33067ddf

![Screenshot 2023-09-29 145717](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/05659b82-ea99-4962-8471-dc3bfca1b8d1)

![Screenshot 2023-09-29 150000](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/c58735d4-88cc-4e01-8ff6-5512610db485)

5. Check if the app is accessible on the browser

![Screenshot 2023-09-29 152134](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/7d203cf4-84ac-4b90-baa3-4e1cac776994)

6. Under Build steps, select execute shell and enter the docker commands used

![Screenshot 2023-09-29 152051](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/bb45da4e-ab57-4c21-9550-60c8bfc4489e)

7. Select "Build now". The build will fail with the following output:

   ![image](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/6163fd69-8cfd-4a72-a4d5-d4d8243cca0b)

8. Run the following in the terminal: sudo usermod -a -G root jenkins

   sudo service jenkins restart

   sudo chmod 777 /var/run/docker.sock

   Restart the terminal


10. Select "Build now". The build will fail with the following output:
![Screenshot 2023-09-29 155143](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/3543585b-0740-409f-9520-61b245e82045)

Kill the existing container in the terminal.
![Screenshot 2023-09-29 155338](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/7912e1a2-3cd5-458b-b9ca-c9fe0d80c24e)

Run the build again. This will be successful.
![Screenshot 2023-09-29 155433](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/f31141b9-1fb5-43ae-bfaa-accda6d50d63)

Edit the build steps to include removing the existing container.

![Screenshot 2023-09-29 165201](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/9032f2f1-924f-4e38-ba7c-010d9d4237e8)


9. Since a webhook has been configured and all steps automated, when an update is made in the repo, it will automatically trigger a build. The whole pipeline will run automatically.

![Screenshot 2023-09-29 164910](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/a30b6f95-8154-4be6-84fc-638393575adb)

 
