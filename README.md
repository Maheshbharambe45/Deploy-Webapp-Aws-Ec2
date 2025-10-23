#  Deploy a Web Application on AWS EC2 (Ubuntu AMI)

## Prerequisites
 
- Launched EC2 instance  
- **AMI:** Ubuntu  
- Created Key Pair  
- Created Security Group  


## Step 1: Launch EC2 Instance

1. Choose **Ubuntu AMI**  
2. Select an instance type (e.g., `t2.micro`)  
3. Add **Key Pair**  
4. Create a **Security Group** with inbound rules:
   - **SSH** → Port `22`
   - **HTTP** → Port `80`
5. Launch the instance ✅
 ![Website Screenshot](assets/Screenshot%202025-10-23%20104811.png)

6.Security group 
![Website Screenshot](assets/Screenshot%202025-10-23%20105143.png)



## Step 2: Connect to the EC2 Instance

Use your key pair to SSH into the instance:

```bash
ssh -i "MY-KEY.pem" ec2-user@ec2-43-204-148-151.ap-south-1.compute.amazonaws.com
```
![Website Screenshot](assets/Screenshot%202025-10-23%20095923.png)

## Step 3: Install Packages and Dependencies

1️⃣ Update and upgrade the system
```bash
sudo apt update -y
sudo apt upgrade -y
```
![Website Screenshot](assets/Screenshot%202025-10-23%20101527.png)

2️⃣ Install Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```
![Website Screenshot](assets/Screenshot%202025-10-23%20101749.png)
![Website Screenshot](assets/Screenshot%202025-10-23%20101808.png)

3️⃣ Install Git and Nginx
```bash
sudo apt install -y git nginx
```
![Website Screenshot](assets/Screenshot%202025-10-23%20101836.png)

4️⃣ Check installations
```bash
git --version
node --version
```
![Website Screenshot](assets/Screenshot%202025-10-23%20101925.png)

## Step 4: Deploy Your Application

```bash
git clone https://github.com/Maheshbharambe45/Deploy-Webapp-Aws-Ec2.git
cd Deploy-Webapp-Aws-Ec2
```


project has dependencies, install them

```bash
npm install
```

## Step 5: Add Firewall Rules

1️⃣ Install firewalld
```bash
sudo yum install -y firewalld
```

2️⃣ Enable and start the firewall service
```bash
sudo systemctl enable firewalld
sudo systemctl start firewalld
```
![Website Screenshot](assets/Screenshot%202025-10-23%20102031.png)
![Website Screenshot](assets/Screenshot%202025-10-23%20102050.png)

3️⃣ Allow HTTP (port 80) and HTTPS (port 443)
```bash
sudo firewall-cmd --permanent --add-service=http

```

4️⃣ Reload firewall to apply changes
```bash
sudo firewall-cmd --reload
```

## Step 6: Set Up Reverse Proxy Using Nginx

Edit the Nginx configuration:
![Website Screenshot](assets/Screenshot%202025-10-23%20102317.png)

```bash
sudo nano /etc/nginx/nginx.conf
```

Add this block inside the http block

```bash
server {
    listen 80;
    server_name your-ec2-public-ip;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

restart nginx

```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

## Step 7: Start Your Application

```bash
node index.js
```
![Website Screenshot](assets/Screenshot%202025-10-23%20102439.png)

## Step 8: Access Your Application

```bash
http://<your-ec2-public-ip>
```
![Website Screenshot](assets/Screenshot%202025-10-23%20102455.png)

![Website Screenshot](assets/Screenshot%202025-10-23%20102540.png)
