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
- **Phase 5:** Enable HTTPS with Cloudflare Origin CA and Nginx Reverse Proxy

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

### Task-2: Deploying React Frontend Application

#### Steps:

1. Goto the `frontend` directory, create `.env` file and insert the connection to backend

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

2. Once the file is created, install necessary packages and run the Frontend Application

    ```sh
    npm install
    npm start
    ```

    <img width="1707" height="366" alt="image" src="https://github.com/user-attachments/assets/64f07691-06e4-4e0f-9c14-357de51f7fe0" />
    <img width="468" height="326" alt="image" src="https://github.com/user-attachments/assets/9dba5511-255c-48f0-b6b5-ff26ade7f50d" />

3. Verify if the Frontend Application is running, using the browser `http://<EC2-PUBLIC-IP>:3000`

    <img width="1366" height="117" alt="image" src="https://github.com/user-attachments/assets/5c6349e4-be6c-4e73-80d8-997689888a24" />

### Task-3: Verify the application deployment

<img width="1707" height="979" alt="image" src="https://github.com/user-attachments/assets/c4de274e-b023-4c12-b0e4-f23a96f05076" />
<img width="1707" height="979" alt="image" src="https://github.com/user-attachments/assets/dc4af0f9-e24b-4d3d-b91a-a4d0812e2607" />

---

## Phase 3: Scaling & Load Balancing

### Task-1: Create an AMI of the EC2 Instance

#### Steps:

1. Navigate to the EC2 Console, select the EC2 instance and click on **Actions** dropdown in the **Image and templates** > **Create Image**

    <img width="1438" height="281" alt="image" src="https://github.com/user-attachments/assets/614f6692-c175-4826-afdf-8e0a1c3d5292" />

2. Enter the **Image name** and **Image description**

    <img width="1342" height="837" alt="image" src="https://github.com/user-attachments/assets/7f514a64-6128-4992-9411-2c6cfeb3b965" />

3. Click on **Create image**
4. On the left sidebar, click on AMIs and check if the AMI is created successfully and status shows **Available**

    <img width="1441" height="166" alt="image" src="https://github.com/user-attachments/assets/92d23b18-2381-43ef-b8f5-24f653d46d56" />

### Task-2: Create a Launch Template

#### Steps:

1. From the left sidebar, click on **Launch Templates** under **Instances**, click on **Create launch template**

    <img width="1689" height="637" alt="image" src="https://github.com/user-attachments/assets/a7e73fcf-9de4-4fdb-bee3-bd21b5982bdb" />

2. Enter the name of the **Launch template name** and **Template version description**

    <img width="1138" height="411" alt="image" src="https://github.com/user-attachments/assets/3a2f2d7c-103b-4c15-ba9e-660bdf82fcd4" />

3. Under the **Launch template contents** section, click on **My AMIs** and select the AMI created in previous Task.

    <img width="1125" height="713" alt="image" src="https://github.com/user-attachments/assets/f054cdce-a07b-4e4d-91d1-58f47a29f309" />

4. Select the **Instance type** as **t3.micro**, select previously created kerpair and Security groups under **Key pair** and **Network settings** section

    <img width="1125" height="815" alt="image" src="https://github.com/user-attachments/assets/0ab86479-f4d8-42cf-956c-c04be661252e" />

5. Then click on **Create launch template**

    <img width="569" height="486" alt="image" src="https://github.com/user-attachments/assets/8fb93798-db43-4c9c-8cb4-fb08d600cc26" />

6. Once done, navigate to the Launch templates and check for the new created template

    <img width="1675" height="79" alt="image" src="https://github.com/user-attachments/assets/cc248649-b785-446c-bf21-ff783c8c1445" />
    <img width="1441" height="154" alt="image" src="https://github.com/user-attachments/assets/0c3a74ec-e6d7-40a3-ade9-d7a1c70bee03" />

### Task-3: Create a Target Group for the ALB

#### Steps:

1. From the left sidebar, navigate to **Target Groups** under **Load Balancing** section
2. Click on **Create target group**

    <img width="1692" height="969" alt="image" src="https://github.com/user-attachments/assets/0a19173b-b64b-4a6c-b102-1dcadc18f1bd" />

3. On the **Create target group** page, under **settings** section select 
    **Target type:** Instances
    **Target group name:** tmmernapp-tg
    **Protocol:** HTTP
    **Port:** 3000

    <img width="1067" height="678" alt="image" src="https://github.com/user-attachments/assets/31071ab2-2f87-4e87-82dd-24df21aacbdc" />

4. Select the **VPC** same as your instance and leave the remaining values to default

    <img width="1123" height="159" alt="image" src="https://github.com/user-attachments/assets/64978124-8ddc-4fa8-8e8f-af7af8110e94" />

5. Click on **Next**
6. Under the **Register targets** section, select the Instance where the application has been hosted and click on **Include as pending below** (Repeat this step if you have multiple instances)

    <img width="1399" height="516" alt="image" src="https://github.com/user-attachments/assets/953247f0-24dd-43f9-a1e1-ecdf110c3877" />

7. Now you can see the targets added under **Review targets**, then click on **Next**

    <img width="1399" height="331" alt="image" src="https://github.com/user-attachments/assets/0d5fce18-8a85-41d2-b5b5-ad9521a65f07" />

8. Click on **Create target group**

    <img width="1399" height="331" alt="image" src="https://github.com/user-attachments/assets/44415dd9-1d20-433b-a50a-4513fe17e4e1" />
    <img width="1435" height="165" alt="image" src="https://github.com/user-attachments/assets/bce3285d-bcfb-4eb4-aa68-2c3267001299" />

### Task-4: Create a Load Balancer (ALB)

#### Steps:

1. From the left sidebar, click on **Load balancers** under **Load balancing** section, click on **Create load balancer**

    <img width="1697" height="832" alt="image" src="https://github.com/user-attachments/assets/ff1ad8ef-db36-4d6e-8bf6-14ff8bb17260" />

2. Click on **Create** under **Application Load Balancer**

    <img width="1022" height="832" alt="image" src="https://github.com/user-attachments/assets/e0780fe4-3b8f-4da3-ac5b-add6c2f3368e" />

3. Enter **Load balancer name** and select **Internet-facing** scheme

    <img width="1646" height="495" alt="image" src="https://github.com/user-attachments/assets/600e7e92-0216-4082-8a7d-acb8578213cb" />

4. Under **Network mapping** section, select the VPC same as your instance and select 2 public subnets in different availability zones

    <img width="1646" height="630" alt="image" src="https://github.com/user-attachments/assets/36f6f827-458d-424a-a3c4-25bbe4e8d4b9" />

5. Under the **Security groups** section, select the same Security group used by the Instance

    <img width="1171" height="227" alt="image" src="https://github.com/user-attachments/assets/12aec935-9dcd-43fc-b91d-ce526afdb060" />

6. Under **Listeners & routing** section, set the parameters as below

    **Listener:**  
           Protocol: HTTP  
           Port: 80  
    **Default action:**  
        Routing action: Forward to target groups  
            Target group: Select the target group created in previous task  
    Leave the remaining values to default

    <img width="1474" height="820" alt="image" src="https://github.com/user-attachments/assets/7bee52d9-e2ef-455b-88ce-ea03dd233f59" />

8. Scroll down and click on **Create load balancer**

    <img width="269" height="66" alt="image" src="https://github.com/user-attachments/assets/1c38a310-4c41-4a1c-a2fb-6c6307b635b2" />

9. Wait till the Load balacer status become **Active**

    <img width="1710" height="939" alt="image" src="https://github.com/user-attachments/assets/8f00b9c8-d477-45ad-b153-59ac7acbcc31" />

### Task-5: Create Auto Scaling Group (ASG)

#### Steps:

1. From the left sidebar, click on **Auto Scaling Groups** under **Auto Scaling** section

    <img width="1483" height="820" alt="image" src="https://github.com/user-attachments/assets/81a393c1-86f1-4066-a7b0-8c6f1e2d9dd2" />

2. Click on **Create Auto Scaling group**

    <img width="397" height="191" alt="image" src="https://github.com/user-attachments/assets/0fd41c6c-1964-424b-b654-1e9b048f97d6" />

3. Enter **Auto Scaling group name**

    <img width="1446" height="330" alt="image" src="https://github.com/user-attachments/assets/79adaa87-a3d3-42df-9315-22f324549c76" />

4. Select the **Launch template** which was created in the previous task and select the **Default** version

    <img width="1371" height="752" alt="image" src="https://github.com/user-attachments/assets/a5fa4974-b33a-41c0-9473-1489dd975958" />

5. Click on **Next**
6. Leave **Instance type requirements** to defaults
7. Under the Networking section, select the VPC same as your instance and select 2 public subnets in different availability zones. Click on **Next**

    <img width="1371" height="701" alt="image" src="https://github.com/user-attachments/assets/8204d5d0-0531-432f-8b53-64f4ebe2b897" />

8. Under **Load balancing options**, select **Attach to an existing load balancer** and **Choose from your load balancer target groups**, then select the target group created in previous task

    <img width="1371" height="505" alt="image" src="https://github.com/user-attachments/assets/22b9f9e9-4043-47a7-9ac3-683f9dc80265" />

9. Leave the remaining to defaults and click on **Next**
10. Under **Group size** section, enter **Desire capacity** as **1**

    <img width="1352" height="274" alt="image" src="https://github.com/user-attachments/assets/0c634881-0633-42ef-bc2a-acc1b1e35fea" />

11. Under **Scaling** section, enter **Min desired capacity** to **1**, **Max desired capacity** to **3**
12. In **Automatic scaling** section, select **Target tracking scaling policy**, set **Metric type** to **Average CPU utilization** and **Target value** to **75**

    <img width="1371" height="762" alt="image" src="https://github.com/user-attachments/assets/4cbc57aa-e82c-4d40-a226-6af5531f6001" />

13. Leave the remaining settings to default and click **Next** until you reach **Review** page
14. Review all the details, scroll down to the bottom and click on **Create Auto Scaling group**

    <img width="428" height="77" alt="image" src="https://github.com/user-attachments/assets/3779a7ed-e234-4f5c-9525-bdcedd8360aa" />

15. Verify that ASG has created

    <img width="1435" height="162" alt="image" src="https://github.com/user-attachments/assets/e1262f6d-7ad5-4109-9dc9-65951962ae54" />

### Task-6: Verify the Load distribution

#### Steps:

1. Navigate to **Traget groups** under **Load balancing** section
2. Select the **Target group**, navigate to **Targets** section in the bottom and verify all the targets are **Healthy**

    <img width="1457" height="681" alt="image" src="https://github.com/user-attachments/assets/61244e43-d0e1-401a-a1e2-0f0077e07bcf" />

---

## Phase 4: Domain Integration via Cloudflare  

### Task-1: Create Cloudflare DNS records

**Pre-requisites**

- Domain (gowthamtanneeru.online)
- ALB DNS Name: tmmernapplb001-1162399349.us-east-1.elb.amazonaws.com

#### Steps:

1. Login to your Cloud flare account

    <img width="1710" height="981" alt="image" src="https://github.com/user-attachments/assets/4a3be70a-676b-411c-a182-a10fb7cd5c75" />

2. Click on the domain name, which navigates to the configuration page as below

    <img width="1710" height="981" alt="image" src="https://github.com/user-attachments/assets/e23fee4c-9305-4b21-8f5e-653f6f891440" />

3. From the left sidebar, expand **DNS** and click on **Records**, then click on **Add record**

    <img width="1283" height="310" alt="image" src="https://github.com/user-attachments/assets/558f899c-e8ea-4e84-952c-3e3bff574921" />

4. Add below Records one at a time and click on **Save**

    **Type:** CNAME
    **Name:** www
    **Target:** tmmernapplb001-1162399349.us-east-1.elb.amazonaws.com
    **Proxy status:** Disabled

    <img width="1228" height="411" alt="image" src="https://github.com/user-attachments/assets/a3671f1c-2d14-4417-a6ff-2a1ccad7cc05" />

    **Type:** A
    **Name:** @
    **Target:** Public_IP of Instance (Elastic IP)
    **Proxy status:** Disabled

    <img width="1228" height="411" alt="image" src="https://github.com/user-attachments/assets/bcf972dc-4507-4305-8417-8ebbf02e1567" />

6. Verify the if the records created

    <img width="1276" height="349" alt="image" src="https://github.com/user-attachments/assets/c30d562d-63a7-4980-a9ea-e87dd6f688e2" />

7. Validate the domain by visiting `http://www.gowthamtanneeru.online` or `http://gowthamtanneeru.online`

    <img width="1500" height="513" alt="image" src="https://github.com/user-attachments/assets/825075c0-b4d6-4a78-b4e8-8cd118b0efb4" />

---

## Phase 5: Enable HTTPS with Cloudflare Origin CA and Nginx Reverse Proxy  

### Task-1: Generate Cloudflare Origin CA Certificate

#### Steps:

1. Navigate to the **Cloudflare Dashboard**
2. Form the Left sidebar, click on **Origin Server** under **SSL/TLS** section, click on **Create Certificate**

    <img width="1710" height="983" alt="image" src="https://github.com/user-attachments/assets/fb8fe5d2-1234-4481-b847-63fccdfc3293" />

3. By default all the details are pre-filled, if not enter the details as below

    **Private key type:** RSA  
    **Hostnames:** `yourdomain` & `*.yourdomain`  
    
    <img width="1058" height="744" alt="image" src="https://github.com/user-attachments/assets/a8ad5f80-586e-47f9-a21a-8ff6891a10e5" />

4. Click on Create
5. After the certifates are successfully created, Cloudflare provides two .pem files [Origin Certificate, Private Key]
6. Save them locally as `origin.pem` and `origin.key`
7. Click on **OK**

    <img width="963" height="731" alt="image" src="https://github.com/user-attachments/assets/c8c34f8e-0c7b-4303-b7d3-f5bff84b91b3" />

### Task-2: Upload certificate files to EC2 using SCP

#### Steps:

1. From your local machine copy the files the server

    ```sh
    scp -i <PATH_TO_PEM_KEY> <PATH_TO_PEM>/origin.pem <EC2_USER>@<EC2_PUBLIC_IP>:/home/<EC2_USER>/
    scp -i <PATH_TO_PEM_KEY> <PATH_TO_KEY>/origin.key <EC2_USER>@<EC2_PUBLIC_IP>:/home/<EC2_USER>/
    ```

    - <PATH_TO_PEM> → location where you saved origin.pem
    - <PATH_TO_KEY> → location where you saved origin.key
    - <EC2_USER> → ubuntu if you're using ubuntu image
    - <EC2_PUBLIC_IP> → Elastic IP of your EC2 instance
    - <PATH_TO_PEM_KEY> → Your EC2 Instance private key (You must have executable permissions for this)

    <img width="1710" height="107" alt="image" src="https://github.com/user-attachments/assets/ed6c74a4-c589-49f6-a8b0-32226442bc96" />

### Task-3: Install Origin CA Certificate on EC2

#### Steps:

1. Move the cert files to system paths

    ```sh
    sudo mv origin.pem /etc/ssl/certs/
    sudo mv origin.key /etc/ssl/private/
    ```

    <img width="437" height="32" alt="image" src="https://github.com/user-attachments/assets/1eed53e9-bd18-4740-b7b4-c09fec6f3e43" />

2. Assign permissions to both teh files

    ```sh
    sudo chmod 644 /etc/ssl/certs/origin.pem
    sudo chmod 600 /etc/ssl/private/origin.key
    ```

    <img width="481" height="46" alt="image" src="https://github.com/user-attachments/assets/83d93649-a4cd-4272-bfba-e8431c90fe09" />

3. Assign the owenrship to the files

    ```sh
    sudo chown root:root /etc/ssl/certs/origin.pem
    sudo chown root:root /etc/ssl/private/origin.key
    ```

    <img width="525" height="46" alt="image" src="https://github.com/user-attachments/assets/af608557-852e-421d-a806-61d62b286d6a" />

4. Verify the permissions have been set correctly

    ```sh
    sudo ls -l /etc/ssl/certs/origin.pem
    sudo ls -l /etc/ssl/private/origin.key
    ```

    <img width="525" height="75" alt="image" src="https://github.com/user-attachments/assets/1c756180-4e0c-497d-abce-3a8cc8e32cfd" />

### Task-4: Configure Nginx for SSL + Reverse Proxy

#### Steps:

1. Install nginx on the EC2 instance

    ```sh
    sudo apt install nginx -y
    ```

    <img width="872" height="631" alt="image" src="https://github.com/user-attachments/assets/3a75cc1c-0f67-4bfd-b77a-ffb4dde45b58" />

2. sd
3. v
4. sd
5. d s
---

## Issues observed

### 1. Application Stops When Pressing `Ctrl+C` During Deployment

Once the frontend and backend has been deployed and run, the application stops as soon as we press `ctrl+c`. This occurs because, node `index.js` and `npm start` run in the foreground of SSH session. Inorder to get rid of this issue, have implemented the below solution,

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

### 2. Public IPv4 Address Keeps Changing After Restarting the EC2 Instance

When the EC2 instance was stopped and started multiple times, the public IPv4 address changed each time. This caused deployment interruptions because:
- The frontend .env variable (REACT_APP_BACKEND_URL) became invalid
- Backend test URLs such as http://<IP>:3001/hello stopped working

Goto EC2 console, in the left sidebar click on **Elastic IPs** under **Network & Security**
- Click on **Allocate Elastic IP address**
    <img width="1471" height="109" alt="image" src="https://github.com/user-attachments/assets/5aaee7ed-ed57-4ba8-828a-10a0792d5079" />
- Click on **Allocate**
    <img width="1667" height="676" alt="image" src="https://github.com/user-attachments/assets/7d089530-8d0d-497b-bdd8-5593c3612a27" />

Once the IP is created, 
- Click on the IP

    <img width="1444" height="132" alt="image" src="https://github.com/user-attachments/assets/685d8b8b-b459-43e1-9a71-40f85d3a1571" />
- Click on **Associate Elastic IP address**

    <img width="1444" height="132" alt="image" src="https://github.com/user-attachments/assets/89ec0918-ab11-46b8-8105-ace6cee8134b" />
- Make sure **Resource Type** as **Instance**, then select required **Instance** and **Private IP address** of the instance

    <img width="1665" height="551" alt="image" src="https://github.com/user-attachments/assets/0ba4823c-9ff7-4dab-adfe-7ee89161af99" />
- Click on **Associate**

    <img width="1442" height="56" alt="image" src="https://github.com/user-attachments/assets/e76e38ee-4405-4d01-a766-0ac38071a28b" />

Login to the EC2 instance, restart frontend service with pm2

```sh
pm2 restart travelmemory-frontend
```

<img width="1069" height="170" alt="image" src="https://github.com/user-attachments/assets/49d4eb2b-1e7a-4849-9f43-586bc52b0b3a" />

You can see the application be working fine.
