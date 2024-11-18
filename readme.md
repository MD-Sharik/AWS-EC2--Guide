# Steps to Set Up EC2, Deploy Backend, and Generate SSL

## 1. **Start an EC2 Instance**
   - Launch a new EC2 instance on AWS.
   - Choose an appropriate instance type and OS (e.g., Ubuntu 20.04).
   - Configure security groups to allow necessary ports (e.g., 22 for SSH, 80 for HTTP, 443 for HTTPS).
   - Allocate and associate an Elastic IP if needed.
   - **Command Example**:
     ```bash
     aws ec2 run-instances --image-id ami-xxxxx --count 1 --instance-type t2.micro --key-name YourKeyName --security-group-ids sg-xxxxx --subnet-id subnet-xxxxx
     ```

---

## 2. **Connect to the EC2 Instance**
   - Access the instance using an SSH client.
   - Ensure you have the appropriate private key file to connect.
   - **Command Example**:
     ```bash
     ssh -i "YourKey.pem" ubuntu@your-ec2-public-ip
     ```

---

## 3. **Update and Prepare the Server**
   - Update the package manager.
   - Install necessary packages and dependencies for the backend.
   - **Command Example**:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install -y nginx git nodejs npm
     ```

---

## 4. **Upload Your Backend Application**
   - Transfer your backend code to the EC2 instance using a secure file transfer method.
   - **Command Example**:
     ```bash
     scp -i "YourKey.pem" -r /path/to/backend-code ubuntu@your-ec2-public-ip:/path/on/ec2
     ```

---

## 5. **Configure the Backend**
   - Install dependencies and set up the backend.
   - Start the backend server.
   - **Command Example**:
     ```bash
     cd /path/to/backend
     npm install
     node app.js
     ```

---

## 6. **Configure Nginx as a Reverse Proxy**
   - Set up Nginx to proxy requests to your backend application.
   - Test and reload the Nginx configuration.
   - **Command Example**:
     ```bash
     sudo nano /etc/nginx/sites-available/default
     sudo nginx -t
     sudo systemctl restart nginx
     ```

---

## 7. **Install Certbot and Generate SSL Certificate**
   - Install Certbot and obtain an SSL certificate for your domain.
   - Link the certificate to Nginx.
   - **Command Example**:
     ```bash
     sudo apt install certbot python3-certbot-nginx -y
     sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
     ```

---

## 8. **Verify SSL Installation**
   - Confirm that HTTPS is working for your domain.
   - Check the SSL certificate's validity.
   - **Command Example**:
     ```bash
     curl -I https://yourdomain.com
     ```

---

## 9. **Push Backend Code to GitHub**
   - Initialize a Git repository if not already done.
   - Push your backend code to a GitHub repository.
   - **Command Example**:
     ```bash
     git init
     git remote add origin https://github.com/yourusername/your-repo.git
     git add .
     git commit -m "Initial commit"
     git push -u origin main
     ```
