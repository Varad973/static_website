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

OR

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AWS Ubuntu Website</title>

    <style>
        *{
            margin:0;
            padding:0;
            box-sizing:border-box;
            font-family: Arial, sans-serif;
        }

        body{
            height:100vh;
            display:flex;
            justify-content:center;
            align-items:center;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            overflow:hidden;
        }

        .container{
            text-align:center;
            background: rgba(255,255,255,0.12);
            padding:50px;
            border-radius:20px;
            backdrop-filter: blur(10px);
            box-shadow:0 8px 25px rgba(0,0,0,0.3);
            color:white;
            width:80%;
            max-width:700px;
            animation: fadeIn 1.5s ease;
        }

        h1{
            font-size:50px;
            margin-bottom:20px;
        }

        p{
            font-size:20px;
            margin-bottom:30px;
            line-height:1.6;
        }

        .btn{
            display:inline-block;
            padding:15px 30px;
            background:white;
            color:#2a5298;
            text-decoration:none;
            font-size:18px;
            border-radius:50px;
            transition:0.3s;
            font-weight:bold;
        }

        .btn:hover{
            background:#ffcc00;
            color:black;
            transform:scale(1.05);
        }

        @keyframes fadeIn{
            from{
                opacity:0;
                transform:translateY(30px);
            }
            to{
                opacity:1;
                transform:translateY(0);
            }
        }

        .circles div{
            position:absolute;
            border-radius:50%;
            background:rgba(255,255,255,0.1);
            animation: float 10s infinite linear;
        }

        .circles div:nth-child(1){
            width:120px;
            height:120px;
            left:10%;
            top:20%;
        }

        .circles div:nth-child(2){
            width:200px;
            height:200px;
            right:15%;
            top:10%;
        }

        .circles div:nth-child(3){
            width:150px;
            height:150px;
            left:20%;
            bottom:10%;
        }

        .circles div:nth-child(4){
            width:100px;
            height:100px;
            right:25%;
            bottom:15%;
        }

        @keyframes float{
            0%{
                transform:translateY(0px) rotate(0deg);
            }
            100%{
                transform:translateY(-30px) rotate(360deg);
            }
        }
    </style>
</head>

<body>

    <div class="circles">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>

    <div class="container">
        <h1>Welcome to AWS EC2</h1>

        <p>
            Your Ubuntu static website is successfully hosted on an 
            AWS EC2 instance.
            <br><br>
            Fast • Secure • Cloud Powered
        </p>

        <a href="#" class="btn">Explore More</a>
    </div>

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
