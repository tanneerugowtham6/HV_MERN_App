# Travel Memory Application based on MERN Stack

---

The Travel Memory application is a full-stack MERN (MongoDB, Express.js, React, Node.js) project designed to store and retrieve travel experiences. As part of a cloud deployment exercise, you are responsible for deploying the complete application onto AWS using industry-standard DevOps practices.

The objective is to ensure the application is fully operational, scalable, secure, and accessible over a custom domain. This involves configuring compute resources, environment variables, networking, load balancing, auto-scaling, DNS routing, and ensuring successful communication between the frontend and backend services.

As a DevOps Engineer, your role is to deploy and scale this application using AWS services while adhering to cloud architecture best practices.


---

## Project Execution Overview

This project is executed in **4 phases**, each containing a set of clear deployment tasks required to fully host and scale the Travel Memory MERN application on AWS.

### Phases of Deployment
- **Phase 1:** Instance Setup & Environment Preparation
- **Phase 2:** Application Deployment (Backend + Frontend)
- **Phase 3:** Scaling & Load Balancing
- **Phase 4:** Domain Integration via Cloudflare

---

## Architecture Diagram

The following diagram represents the complete AWS architecture used to deploy, scale, and route traffic for the Travel Memory MERN application.

---

## Environment

### Cloud Platform
- **Provider:** AWS EC2

### Operating System
- **OS:** Ubuntu 20.04 LTS

### AWS Services Used
- EC2
- AMI
- Launch Template
- Auto Scaling Group (ASG)
- Application Load Balancer (ALB)
- Security Groups
- VPC

### Database
- MongoDB Atlas

### Additional Services
- Cloudflare


## Technology Stack

### Frontend
- React
- JavaScript
- npm

### Backend
- Node.js
- Express.js

### Process Management
- PM2

### Version Control
- Git

---

## Phase 1: Instance Setup & Environment Preparation 

### Task-1: Launching an EC2 Instance for Backend Deployment

#### Steps:

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

### Task-2: Connect to the EC2 Instance

#### Steps:

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

### Task-3: Configure the EC2 Instance

#### Steps:

1. Update and upgrade the available packages

    ```sh
    sudo apt update
    sudo apt upgrade -y
    ```

    <img width="651" height="42" alt="image" src="https://github.com/user-attachments/assets/7e83149d-fee9-48e1-bccd-38c1c11e7894" />
    <img width="677" height="99" alt="image" src="https://github.com/user-attachments/assets/0b2f3b7c-bbbe-47f4-ba43-1e14a557c54b" />
    <img width="1621" height="191" alt="image" src="https://github.com/user-attachments/assets/6fb99eac-0397-4dae-878a-5a00e2eb9e6c" />
    <img width="802" height="213" alt="image" src="https://github.com/user-attachments/assets/05c4118e-674d-4d01-a2a5-e8ba615fae09" />

2. Install Node.js 22 and npm

    ```sh
    curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
    sudo apt install -y nodejs
    ```

    <img width="861" height="1043" alt="image" src="https://github.com/user-attachments/assets/99a28a40-c0f0-4ed9-9cb9-70f1013280cc" />
    <img width="807" height="619" alt="image" src="https://github.com/user-attachments/assets/fa0e0db7-d91a-443f-b24b-75afb231e824" />

3. Verify the installation using below commands

    ```sh
    node -v
    npm -v
    ```

    <img width="255" height="83" alt="image" src="https://github.com/user-attachments/assets/f5c2032e-1f11-4b11-a918-699a798be966" />

### Task-4: Configure the MongoDB

#### Steps:

1. Sign in to your MongoDB Atlas account (https://www.mongodb.com/cloud/atlas)
2. Once logged in, Click on `Create a cluster`

    <img width="1204" height="578" alt="image" src="https://github.com/user-attachments/assets/88a8357c-78ed-4b35-aa26-328495bbe0c6" />

3. Select the required plan, since it's a demo going with a `free` plan

    <img width="1419" height="483" alt="image" src="https://github.com/user-attachments/assets/09dc27b3-acd8-4c6b-a459-f9fe9df443b3" />

4. Enter the required details under the **Configurations** section like, **Name, Provider, Region**

    <img width="708" height="504" alt="image" src="https://github.com/user-attachments/assets/86805543-023a-476d-8fad-55609e63fe87" />

5. Click on **Create Deployment**

    <img width="513" height="91" alt="image" src="https://github.com/user-attachments/assets/28f40b1f-32dc-49dd-b2bd-eb19f26a4bd1" />

6. Once created, Connect to <your-cluster-name> window will be popped up. Click on **Compass**

    <img width="829" height="838" alt="image" src="https://github.com/user-attachments/assets/6d52e18a-926b-45b5-b895-5edfa52041f7" />

7. Select **I have MongoDB Compass installed**, copy the connection string [Save this for later steps]

    <img width="829" height="856" alt="image" src="https://github.com/user-attachments/assets/de0720af-df4b-4c55-9a5f-486360f4b99b" />

    **NOTE:** If you do not have MongoDB Compass installed, download it from https://downloads.mongodb.com/compass/mongodb-compass-1.48.2-darwin-arm64.dmg

8. Click on **Done**

9. On the left sidebar, click on **Database & Network Access**

    <img width="469" height="697" alt="image" src="https://github.com/user-attachments/assets/65cb8e4e-4cb7-4c98-a8b5-bbc1e40421e8" />

10. Click on **ADD NEW DATABASE USER**

    <img width="1440" height="113" alt="image" src="https://github.com/user-attachments/assets/925f3321-40fd-40c0-bba3-323013dcba1b" />

11. Enter a username and password

    <img width="978" height="599" alt="image" src="https://github.com/user-attachments/assets/197c01f6-db9c-4226-bb8b-eaf2bf93f9c0" />

12. Goto **Database User Privileges** section, under **Built-in Role**, select **Read and write to any database**. Click on **Add User**

    <img width="953" height="720" alt="image" src="https://github.com/user-attachments/assets/676d6a56-996e-4b62-9eff-a5761c065813" />

13. Clcik on **IP Access List** on the left sidebar. Click on **ADD IP ADDRESS**

    <img width="1433" height="109" alt="image" src="https://github.com/user-attachments/assets/79fa3f63-ad1f-4760-a9df-514639826637" />
    <img width="613" height="474" alt="image" src="https://github.com/user-attachments/assets/7b815764-4fc2-4120-b5a6-864d89727cb6" />

14. Click on **Save Changes**

### Task-5: Connect to Database using MongoDB Compass

#### Steps:

1. Launch MongoDB Compass, click on **Add new connection**

    <img width="1424" height="747" alt="image" src="https://github.com/user-attachments/assets/23dbfac0-9624-4d06-afc9-f56a054ddcc8" />

2. Enter the **Connection string** copied from the previous task in the **New Connection** tab under **URI**

    <img width="1921" height="1000" alt="image" src="https://github.com/user-attachments/assets/00d151c1-5abb-4b00-a283-16dca6503218" />

    Replace <db_password> with your password created in previous task.
   
4. Click on **Save & Connect**
5. In the **Connections** tab on Left sidebar, hover the mouse on cluster name, then click on **+** to **Create database**

    <img width="329" height="272" alt="image" src="https://github.com/user-attachments/assets/abc05e04-a167-4b62-b0d1-a7d2311b8f44" />

6. Enter the Database details, **Database Name, Collection Name** then click on **Create Database**

    <img width="609" height="480" alt="image" src="https://github.com/user-attachments/assets/7ca63b8a-8993-494a-9062-59a6d31452be" />

7. Save your MongoDB Connection URI, as this may require in further configuration.

---

## Phase-2: Application Deployment (Backend + Frontend)

### Task-1: Deploying Node.js Backend Application

#### Steps:

1. Goto the backend EC2 instance, clone the repository `https://github.com/UnpredictablePrashant/TravelMemory.git` and verify

    ```sh
    git clone https://github.com/UnpredictablePrashant/TravelMemory.git
    ls -la
    ```
    
    <img width="651" height="317" alt="image" src="https://github.com/user-attachments/assets/94d7e4a1-615c-4200-a559-47ddbcfdb2e4" />
    

2. Goto the `backend` directory, create `.env` file and insert your MongoDB URI [Refer to the files in this repository]

    ```
    cd TravelMemory/backend/
    nano .env
    ```

    Refer `.env` file from `backend` folder in this repository.

    ```
    
    ```

    <img width="694" height="64" alt="image" src="https://github.com/user-attachments/assets/ebfc815b-7f08-4d34-8289-9008c91b38c3" />
    
    Save the file and Exit [`Ctrl+x` & `Enter` (or) `control+x` & `return`]

    <img width="406" height="33" alt="image" src="https://github.com/user-attachments/assets/0382d300-c895-441c-ab96-1b6e3c6f7276" />

3. Once the file is created, install necessary packages and run the Backend Application

    ```sh
    npm install
    node index.js
    ```

    <img width="500" height="332" alt="image" src="https://github.com/user-attachments/assets/23630dc8-4329-4022-9788-4c4d41f89620" />
    <img width="500" height="50" alt="image" src="https://github.com/user-attachments/assets/8917e098-03c1-41ee-a600-76453a45ba81" />

4. Verify if the Backend Application is running, using the browser `http://<EC2-PUBLIC-IP>:3001/hello`

    <img width="1249" height="78" alt="image" src="https://github.com/user-attachments/assets/f1765ad5-63b8-45e9-af46-1c83bc0859a9" />

5. Goto the `frontend` directory, create `.env` file and insert the connection to backend

    ```
    cd TravelMemory/frontend
    nano .env
    ```
    <img width="355" height="19" alt="image" src="https://github.com/user-attachments/assets/42d1dc13-cb94-4de7-a1bd-4420c03e142e" />
    <img width="399" height="19" alt="image" src="https://github.com/user-attachments/assets/3b38790b-0759-47ad-8c9b-57ebdab0d183" />

    Refer `.env` file from `frontend` folder in this repository.

    ```
    REACT_APP_BACKEND_URL=http://EC2-PUBLIC-IP:3001
    ```

    <img width="361" height="38" alt="image" src="https://github.com/user-attachments/assets/3dbc61ce-1971-4e7b-9901-d6137c160f57" />

    Save the file and Exit [`Ctrl+x` & `Enter` (or) `control+x` & `return`]

6. Once the file is created, install necessary packages and run the Frontend Application

    ```sh
    npm install
    npm start
    ```

    <img width="1707" height="366" alt="image" src="https://github.com/user-attachments/assets/64f07691-06e4-4e0f-9c14-357de51f7fe0" />
    <img width="468" height="326" alt="image" src="https://github.com/user-attachments/assets/9dba5511-255c-48f0-b6b5-ff26ade7f50d" />

7. Verify if the Frontend Application is running, using the browser `http://<EC2-PUBLIC-IP>:3000`

    <img width="1366" height="117" alt="image" src="https://github.com/user-attachments/assets/5c6349e4-be6c-4e73-80d8-997689888a24" />

---

## Verify the application

<img width="1707" height="979" alt="image" src="https://github.com/user-attachments/assets/c4de274e-b023-4c12-b0e4-f23a96f05076" />
<img width="1707" height="979" alt="image" src="https://github.com/user-attachments/assets/dc4af0f9-e24b-4d3d-b91a-a4d0812e2607" />

---

## Issues observed

1. Once the frontend and backend has been deployed and run, the application stops as soon as we press `ctrl+c`

    This occurs because, node `index.js` and `npm start` run in the foreground of SSH session.
    Inorder to get rid of this issue, have implemented the below solution,

    ```sh
    sudo npm install -g pm2
    # Verify the installation of pm2
    pm2 -v
    ```

    <img width="499" height="185" alt="image" src="https://github.com/user-attachments/assets/71199eb7-2cd6-4b74-89d4-6160851b2708" />
    <img width="499" height="631" alt="image" src="https://github.com/user-attachments/assets/fc141913-ff65-4457-a893-9a070d7a8eb0" />

    Navigate to `TravelMemory/backend` folder and run the `inedx.js` application as below,

    ```sh
    cd ~/TravelMemory/backend
    pm2 start index.js --name travelmemory-backend
    ```

    <img width="1059" height="132" alt="image" src="https://github.com/user-attachments/assets/67208aeb-4883-4b3a-8dfb-376f78d2ac0b" />
    

    Verify if it's running (if output wasn't visible in previos step)

    ```sh
    pm2 status
    ```

    <img width="1059" height="106" alt="image" src="https://github.com/user-attachments/assets/23c6d433-bcc8-4d40-8ae3-a2527a094b0e" />

    Navigate to `TravelMemory/backend` folder and run the frontend application as below,

    ```sh
    cd ~/TravelMemory/frontend
    pm2 start npm --name travelmemory-frontend -- start
    ```

    <img width="1067" height="152" alt="image" src="https://github.com/user-attachments/assets/7397fe95-a91c-492f-9eac-6ecd9dadcbea" />

    Verify if it's running (if output wasn't visible in previos step)

    ```sh
    pm2 status
    ```

    <img width="1067" height="125" alt="image" src="https://github.com/user-attachments/assets/55805e8e-2c1f-4a97-a74a-764f9f1acfbd" />

    Make the pm2 to auto start these applications on reboot

    ```sh
    pm2 save
    pm2 startup
    ```

    Once ths startup command executed, **as shown in the above screenshot**, execute the below command or copy and paste the command from the SSH session.

    <img width="853" height="337" alt="image" src="https://github.com/user-attachments/assets/586b265a-f92c-429b-843d-ebe67e7f9058" />

    Finally, save the pm2 processes.

    ```
    pm2 save
    ```

    
