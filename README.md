# Docker 3-Tier Student Registration System

##  Project Overview
This project demonstrates a **3-tier web application architecture** using Docker and Docker Compose.

It includes:
-  **Web Layer (Nginx)** – Serves frontend and routes requests  
-  **Application Layer (PHP-FPM)** – Processes form data  
-  **Database Layer (MySQL)** – Stores user data  

The application allows users to fill out a **student registration form**, which is processed and stored in a MySQL database.

---

##  Architecture

                 🌐 USER (Browser)
                        │
                        │  HTTP (Port 80)
                        ▼
        ┌─────────────────────────────────┐
        │        🟩 NGINX (WEB)           │ 
        │   Container: web                │
        │   - Serves signup.html          │
        │   - Reverse Proxy               │
        └──────────────┬──────────────────┘
                       │ FastCGI
                       ▼
        ┌──────────────────────────────────┐
        │        🟨 PHP-FPM (APP)         │
        │   Container: app                │
        │   - Processes submit.php        │
        │   - Handles business logic      │
        └──────────────┬──────────────────┘
                       │ MySQL (Port 3306)
                       ▼
        ┌──────────────────────────────────┐
        │        🟥 MYSQL (DB)            │
        │   Container: db                 │
        │   - Stores user data            │
        │   - users table (FCT DB)        │
        └──────────────────────────────────┘


        🔗 NETWORKS

        ┌──────────────────────────────┐
        │        webnet                │
        │   Nginx  ↔  PHP              │
        └──────────────────────────────┘

        ┌──────────────────────────────┐
        │        dbnet                 │
        │   PHP  ↔  MySQL              │
        └──────────────────────────────┘


---

##  Network Design

- **webnet**
  - Connects: Nginx ↔ PHP
- **dbnet**
  - Connects: PHP ↔ MySQL

This ensures **secure and isolated communication** between layers.

---

##  Project Structure
![](/img/Screenshot%202026-04-29%20142224.png)


---

##  Technologies Used

- Docker
- Docker Compose
- Nginx
- PHP (PHP-FPM)
- MySQL
- HTML/CSS

---

##  Setup & Installation

### Step 1: Create Project Directory

```bash
sudo -i
mkdir /root/3-tier-project
cd /root/3-tier-project
```

---

### Step 2: Create Folder Structure

```bash
mkdir web app db
mkdir web/code web/config
mkdir app/code
```
![](/img/Screenshot%202026-04-29%20142224.png)

---

### Step 3: Frontend (HTML)

 Create file:

```bash
nano web/code/signup.html
```

 Paste your frontend form code (you can use your custom styled UI)

---

### Step 4: Nginx Configuration

Create file:

```bash
nano web/config/default.conf
```

 Configure:

* Serve static HTML from `/usr/share/nginx/html`
* Forward `.php` requests to **app container (FastCGI)**

---

###  Step 5: Backend (PHP)

 Create file:

```bash
nano app/code/submit.php
```

 Add PHP code to:

* Receive form data
* Connect to MySQL (`host = db`)
* Insert into database

---

###  Step 6: PHP Dockerfile

 Create file:

```bash
nano app/Dockerfile
```


 Key points:

* Use `php:7.4-fpm`
* Install `mysqli` extension
* Set working directory `/app`

---

### Step 7: Database Setup

###  Create SQL file:

```bash
nano db/init.sql
```
![](/img/Screenshot%202026-04-29%20141734.png)

 Add:

* Create database (FCT)
* Create table (users)

---

###  Create DB Dockerfile:

```bash
nano db/Dockerfile
```

![](/img/Screenshot%202026-04-29%20141705.png)

 Configure:

* MySQL image
* Root password
* Copy `init.sql`
* Expose port 3306

---

###  Step 8: Docker Compose

 Create file:

```bash
nano docker-compose.yml
```

 Define 3 services:

###  web

* nginx image
* port 80
* mount HTML + config
* connect to `webnet`

###  app

* build from `./app`
* mount PHP code
* connect to `webnet` + `dbnet`

###  db

* build from `./db`
* volume for persistence
* connect to `dbnet`

---

### Step 9: Run Project

```bash
docker compose up -d --build
```
![](/img/Screenshot%202026-04-29%20142350.png)
![](/img/Screenshot%202026-04-29%20142406.png)

---

### Step 10: Access Application

Open in browser:

```
http://<your-server-ip>
```

![](/img/Screenshot%202026-04-29%20142542.png)


![](/img/Screenshot%202026-04-29%20143440.png)
---

### Step 11: Verify Database

```bash
docker exec -it <db-container> mysql -u root -p
```
![](/img/Screenshot%202026-04-29%20143922.png)

Then:

```sql
USE FCT;
SELECT * FROM users;
```

---

### Step 12: Stop Project

```bash
docker compose down
```

---






