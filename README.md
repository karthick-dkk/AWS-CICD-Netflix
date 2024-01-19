<<<<<<< HEAD
# AWS-CICD-Netflix
Netflix app deployment using CI/CD on AWS Cloud
=======
# AWS-DevOps-Project

![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/3598abe3-227e-48a3-94d8-7dc6241d8d0a)

> ### As part of this project, we will create a CI/CD pipeline using AWS tools and deploy the Netflix Application on an EC2 Instance. 
We will use:
1. `CodeCommit`: For Source Code Management
2. `CodeBuild`: For building and testing our code in a serverless fashion
3.  `CodeDeploy`: To deploy our code
4.  `CodePipeline`: To streamline the CI/CD pipeline

---
## 1. Setting Up Source Code Repository in CodeCommit 

1. Open your AWS account.
2. Search for AWS CodeCommit.
3. Create a new repository.
   ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/630500c6-b519-442e-bc54-dd281c7a4fe2)
4. We will use SSH to connect to our repository; for the same, we have to create an AWS User with permission to access this repository.
     1. Go to the IAM console and create a user.
     2. Click on create user.
      ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/c4cafbcb-7a0a-49b4-9caa-bb919dc5f5b1)
     3. Add Permission for full access to CodeCommit.
      ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/4bcd389c-33ae-4abc-8afa-883479172b91)
     4. Click on create for the user.
     5. Click on the user and go to the security credentials section.
     6. Now we are going to create SSH credentials for this user.
     7. Go to the terminal and run this command  
        ```
         ssh-keygen
        ```
     8. Keep all the default values.
     9. Copy the public key using `cat ~/.ssh/id_rsa.pub`.
     10. And paste it into the security credentials, SSH public key for CodeCommit section, and copy the SSH key id.
     11. Go back to the repository and copy the URL for git connection.
     12. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/0f895930-2429-4462-9793-91ff11399bfb)
     13.  ```
          cd ~/.ssh
          touch config
          add these lines in the config file
          Host git-codecommit.*.amazonaws.com
              User <paste SSH key id that you copied earlier>
              IdentityFile ~/.ssh/id_rsa

          ```
     13. Now we can connect to this repo.
     14. Run this command now
          ```
          git clone <SSH key URL>
          ```
     15. Now copy all the content from my git repository to your git repository.
     16. And do a git push.
  
  ---
## 2. Testing and Building the Image using CodeBuild

  1. Click on build project.
  2. And copy these configuration options.
     ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/e096de5b-41c0-476d-b63b-254cda1a6b7b)
     ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/566fe040-8b7e-4958-84e3-6dfc4a55d5ef)
     ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/d30308a5-28c9-49e3-9b17-d7df91066fef)
     ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/658930d2-d25e-498f-a270-846281cd1627)
  3. Click on create build project.

---
## 3. Setting Up Code Pipeline and CodeDeploy

   1. Go to the CodeDeploy Console.
   2. Click on create application.
   3. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/f5810d03-1cd4-4a92-b2ab-17d14031aee1)
   4. Now create a deployment group by clicking.
   5. We have to now create a role for the deployment group; this role should have permission to access `codedeploy`, `s3`.
   6. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/35334675-e8cd-414d-b1eb-cc050a1142d4)
   7. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/6af1d51e-e7f8-40c9-a6f4-283d64e0c741)
   8. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/bb3124b6-478d-47ee-9319-0bc58a688dc6)
   9. Click on Create deployment group.
   10. Now we have to create an EC2 instance, with CodeDeploy agent and Docker, and it should have the name `codeDeploy` <- This should be exactly the same.
   11. Create an EC2 instance, SSH into it, and run these commands; I am using Ubuntu, with all default settings + security group allowing public traffic on port 8080.
   12.  ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/95bce315-31b4-480c-aed7-f91045d10f25)
   13.  ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/7b8fa1ed-8141-4b47-809a-29c2d2f8e43d)
   14.  Now we have to configure the EC2 instance.
   15. Run these commands
   16.  ```
         sudo apt update
         sudo install docker.io
        sudo apt install ruby-full wget
        cd /home/ubuntu
        wget https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install
         chmod +x ./install
         sudo ./install auto
        sudo service codedeploy-agent status
        ```

   ---

  1. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/b585fb1f

-7617-4d24-be98-3ede385c4edd)
  2. Set Up Source Provider as CodeCommit.
  3. Select the repo we have created and the master branch.
  4. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/c5cfb4dc-62fd-439d-bba1-1fae7dfa9d1e)
  5. Now set up code build.
  6. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/5541538a-be4c-4b0e-9566-37ab7f980c88)
  7. Now set up CodeDeploy with all the values we have specified while creating CodeDeploy.
  8. Click on Create pipeline.
  9. After Creating the pipeline, the Pipeline will start automatically.
  10. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/817c9729-b785-4444-9109-cd9042e8a11f)
  11. ![image](https://github.com/inderharrysingh/aws-devops-project/assets/112561014/2952c26a-2411-4232-b790-b4b1dbfde2ca)
>>>>>>> d39fc55 (initial commit)
