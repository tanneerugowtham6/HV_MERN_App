# Travel Memory Application based on MERN Stack

---

The Travel Memory application has been developed using the MERN stack. Below are the complete steps for deploying full-stack applications, working with cloud platforms, and ensuring scalable architecture.

---

## Environment

---

## Task-1: Launching an EC2 Instance for Backend Deployment

### Steps:

1. Login to your AWS Account and search for EC2 service

    <img width="949" height="187" alt="image" src="https://github.com/user-attachments/assets/c899ca60-e19f-4ba9-9e5c-30015013e660" />

2. Click on `Launch Instance`

    <img width="297" height="138" alt="image" src="https://github.com/user-attachments/assets/bdcf3af9-b39b-49dc-9535-0809f7018e24" />

3. Enter the name of the instance, select `Ubuntu Server 24.04 LTS` AMI and select the type of the instance

    <img width="973" height="717" alt="image" src="https://github.com/user-attachments/assets/f38bc036-cae2-4f4b-82aa-b686c7dd717c" />

4. Create a new keypair (Optional) or choose an existing keypair

    <img width="555" height="529" alt="image" src="https://github.com/user-attachments/assets/85ca4ac6-9f31-47ae-929a-ec4502f56cad" />

5. Under Networks Settings, click on `edit`. Select the required VPC and a Public Subnet. Make sure **Auto-assign Public IP** is set to `Enable`

    <img width="819" height="270" alt="image" src="https://github.com/user-attachments/assets/1863c06c-f5c2-4173-9bda-7bd72c3a1a2b" />

6. Under the **Firewall (security groups)**, select **Create security group**. Enter the required details, **Security group name, Description**

    <img width="940" height="250" alt="image" src="https://github.com/user-attachments/assets/2491830e-cf70-4a69-baef-17e558aac9c7" />

7. Allow the ports `80, 443, 3000, 3001` or `HTTP, HTTPS, or Custom TCP` as below using **Add security group rule** button

    <img width="795" height="820" alt="image" src="https://github.com/user-attachments/assets/602e22a2-21f1-4578-a5ff-3319bda2d0e9" />

8. Click on the `Launch instance` button, which initiates the instance. Refer below for the confirmation

    <img width="689" height="220" alt="image" src="https://github.com/user-attachments/assets/d8b66802-7138-4aed-b493-f96fa19e0cee" />

---

## Task-2: Connect to the Backend EC2 Instance

### Steps:

1. Go to the EC2 Instance and copy Public IPv4 or Public DNS
2. Launch the Terminal (macOS), Command Prompt (Windows)
3. Navigate to the folder where your `.pem` file is downloaded and assign the executable permissions to the file

    ```sh
    cd <.pem_file_location>
    chmod 700 tmmern_keypair.pem
    ```

    <img width="556" height="54" alt="image" src="https://github.com/user-attachments/assets/64152aa6-2cf3-47e7-afad-c96441136769" />

4. Connect to the Backend EC2 Instance using the `ssh` command

    ```sh
    ssh -i "<.pem_file_name>" <username>@<public_dns or ipv4 of the instance>
    ```

    <img width="888" height="641" alt="image" src="https://github.com/user-attachments/assets/65770b11-f593-4465-af67-3b6d95118c30" />

---

## Task-3: Configure the Backend EC2 Instance

### Steps:

1. 
