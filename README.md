# Static Website Deployment on AWS EC2 (Ubuntu)

This project demonstrates how to design and deploy a static website on a cloud virtual machine using Amazon EC2. The server is configured to be publicly accessible and supports remote updates using multiple methods.

---

## Steps

### Step 1 — Connect to Ubuntu EC2 Instance

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@<public-ip>
```

> Default username for Ubuntu is `ubuntu`

---

### Step 2 — Update System

```bash
sudo apt update
sudo apt upgrade -y
```

---

### Step 3 — Install Apache Web Server

```bash
sudo apt install apache2 -y
```

Start and enable Apache:

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

### Step 4 — Test Website

Open in browser:

```
http://<your-public-ip>
```

You should see the default Apache page.

---

### Step 5 — Deploy Your Static Website

Navigate to web root:

```bash
cd /var/www/html
```

Remove default file:

```bash
sudo rm index.html
```

Create a new file:

```bash
sudo nano index.html
```

Paste the following HTML:

```html
<!DOCTYPE html>
<html>
<head>
  <title>AWS Ubuntu Website</title>
</head>
<body>
  <h1>Hello from Ubuntu EC2</h1>
</body>
</html>
```

Save and exit:
- Press `Ctrl + X`
- Press `Y`
- Press `Enter`

Refresh browser to view your website.

---

### Step 6 — Fix Permissions

```bash
sudo chown -R ubuntu:ubuntu /var/www/html
```

---

### Step 7 — Configure Security Group

In AWS console, ensure:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80   | 0.0.0.0/0 |
| SSH  | 22   | Your IP   |

---

### Step 8 — Remote Update Methods

#### Method 1: SSH (Direct Editing)

```bash
nano /var/www/html/index.html
```

Edit files directly on the server. Changes reflect instantly on the live website.

---

#### Method 2: SCP (File Transfer)

From your local machine:

```bash
scp -i your-key.pem index.html ubuntu@<public-ip>:/home/ubuntu
```

Move file to web directory:

```bash
mv index.html /var/www/html/
```

---

#### Method 3: Git Deployment

Install Git:

```bash
sudo apt install git -y
```

Clone your repository:

```bash
cd /var/www/html
git clone https://github.com/<your-username>/<repo-name>.git .
```

For updates:

```bash
git pull
```

---

## Conclusion

A static website is successfully deployed on an Ubuntu EC2 instance. The server is publicly accessible and can be updated remotely using SSH, SCP, or Git.
