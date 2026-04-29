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

👉 This ensures **secure and isolated communication** between layers.

---

## 📁 Project Structure
