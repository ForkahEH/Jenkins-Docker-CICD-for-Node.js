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

1. The Jenkins installation instructions from the Jenkins Documentation are followed: https://www.jenkins.io/doc/book/installing/linux/

   Java is installed
   ![Screenshot 2023-09-29 114800](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/aba6d392-8775-42b6-b3a7-6978e8704473)

   Jenkins is installed
   ![Screenshot 2023-09-29 115155](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/bbf2125c-b106-4a7c-91fa-8fe3f1f0d3c2)

3. The Jenkins webserver is accessed from the browser using the public ip and the Jenkins port number 8080
   ![Screenshot 2023-09-29 115309](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/6bb90fe6-8d63-422d-a59a-8337b016b478)

4. The Jenkins server password is retrieved using the command: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
![Screenshot 2023-09-29 115539](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/6fe46a2d-f125-473b-9a2d-e2aa88fd4a24)

5. Login to the Jenkins server
![Screenshot 2023-09-29 115818](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/12a39fd8-aec4-474e-aa24-1bd785188111)

6. Enter the relevant details and voila!! Jenkins is ready.
![Screenshot 2023-09-29 120029](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/068aa685-5950-4606-bdac-64e4e81507c9)

7. Select "New Item", enter the name of the project, select Pipeline and press OK

8. Configure a webhook to allow automatic build of the project
  ![Screenshot 2023-09-29 122206](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/c5808446-830c-4cdb-b584-2d194ef61de6)


9. Enter the configuration details
   Description: CICD pipeline using Jenkins for node.js application
   Project url : https://github.com/ForkahEH/node-todo-cicd.git

10. Enter the pipeline script
![Screenshot 2023-09-29 122614](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/902891ee-e414-424d-a7dc-dc4b4fa3c70f)

11. Select "Build Now"....and the repo is cloned from Github.
![Screenshot 2023-09-29 122848](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/94a1b665-ad91-4ea5-84d3-7a94ba30b0fb)

12. Confirm the repo has been cloned on the EC2 instance.

    ls /var/lib/jenkins/workspace/Jenkins-Docker-CICD-for-Node.js
    ![Screenshot 2023-09-29 123356](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/0fc3f1d3-cdb6-4d93-b620-ae2e36acbec5)

14. Check the readme file and install the necessary dependencies on the EC2 instance.
![Screenshot 2023-09-29 124323](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/30e77be5-4fd7-49d1-b17b-9d0ddecfc66b)

15. The app is running on port 8000.
    In order to access the app on the browser, the Jenkins SG security group settings inbound rules must be edited to allow traffic from port 8000.
    In this case, the source is "Everywhere" since the app would be accessed by people over the internet.
    ![Screenshot 2023-09-29 124819](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/b2b9dba9-cd86-4cbb-ac74-69c7931cde84)


17. The app is accessed from the browser using the public ip and the port number 8000.
![Screenshot 2023-09-29 125342](https://github.com/ForkahEH/Jenkins-Docker-CICD-for-Node.js/assets/127892742/5d02de4c-6a7f-427b-9447-23708794bf57)

18. 
