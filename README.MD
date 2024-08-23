
### **1. Introduction to Cloud and Server**

# Project Repository : https://github.com/ipcs-automation/flask-app.git 

**What is Cloud and Server:**
- **Cloud Computing:** Refers to the delivery of computing services—servers, storage, databases, networking, software—over the internet (“the cloud”). It allows users to access technology services on-demand without owning the physical infrastructure.
- **Server:** A server is a computer system that provides data, services, or programs to other computers, known as clients, over a network.

**Why AWS Cloud is Preferred:**
- **Global Reach:** AWS has a vast network of data centers globally, ensuring low latency and high availability.
- **Scalability:** Easily scale resources up or down depending on demand.
- **Cost-Effective:** Pay-as-you-go pricing model, with various pricing tiers to suit different business needs.
- **Security:** AWS offers robust security features, including data encryption and compliance certifications.
- **Ecosystem:** AWS provides a comprehensive suite of services that integrate seamlessly with each other.

**AWS Account Cost and Plans:**
- **Free Tier:** AWS offers a Free Tier account, allowing new users to explore and use AWS services for free up to certain limits for 12 months.
- **Pay-As-You-Go:** Charges are based on the actual usage of resources, offering flexibility.
- **Savings Plans:** AWS offers long-term contracts for consistent savings on usage.
- **Enterprise Plans:** Customized pricing and services for large organizations.

**How to Create a Free Tier Account:**
- **Step 1:** Go to [AWS Free Tier page](https://aws.amazon.com/free/).
- **Step 2:** Click on “Create a Free Account”.
- **Step 3:** Enter your email, password, and AWS account name.
- **Step 4:** Provide your contact information and payment details (no charges will be incurred if Free Tier limits are not exceeded).
- **Step 5:** Verify your identity via phone or text.
- **Step 6:** Choose the “Basic Support” plan (free).
- **Step 7:** Complete the signup process.

### **2. AWS Console Overview and Key Services**

**AWS Console Home and Basics:**
- **AWS Console:** The management interface for AWS services. You can manage and configure your services, monitor your resources, and access usage reports.
- **Dashboard:** Centralized interface where you can search and navigate AWS services.

**Important AWS Services:**
- **IAM (Identity and Access Management):** Manage user access and encryption keys. Ensure secure and granular control over access to AWS services.
- **EC2 (Elastic Compute Cloud):** Virtual servers in the cloud, allowing you to run applications.
- **S3 (Simple Storage Service):** Scalable object storage for data backup, archiving, and analytics.
- **CDN (Content Delivery Network - CloudFront):** Distributes content globally with low latency.
- **Route 53:** Scalable Domain Name System (DNS) service that routes end-users to Internet applications.
- **RDS (Relational Database Service):** Managed database service for SQL databases.
- **LightSail:** Simplified service for deploying websites and applications with minimal setup.
- **Lambda:** Run code without provisioning or managing servers. Ideal for microservices and short-lived tasks.
- **Billing:** Monitor your usage and manage your costs effectively.

**Setting Up a LightSail Server:**
- **Why LightSail:** Ideal for beginners due to its simplicity and all-in-one pricing.
- **Step 1:** Go to the LightSail section in the AWS Console.
- **Step 2:** Click “Create Instance”.
- **Step 3:** Choose your instance location, platform (Linux/Unix), and blueprint (e.g., OS only).
- **Step 4:** Choose a plan (Start with the lowest if in Free Tier).
- **Step 5:** Name your instance and create it.
- **Networking Setup:** Enable networking, attach a static IP, and configure firewall rules (open ports like 22 for SSH, 80 for HTTP, 443 for HTTPS).
- **Static IP:** Ensures your server retains the same IP address even after a reboot, important for domain mapping and consistency.

**Pem Keys, Plans, and OS:**
- **Pem Keys:** A file used for SSH access to your server. Download this file securely when creating your instance.
- **Plans:** LightSail offers fixed pricing plans, inclusive of compute, storage, and bandwidth.
- **Server OS:** Choose Ubuntu for your Flask app due to its popularity, support, and compatibility with Python.

### **3. Setting Up a Basic Python Flask Application**

**Setting Up Flask:**
- **Step 1:** SSH into your LightSail instance using the `.pem` key.
- **Step 2:** Install Python and Flask (`sudo apt update && sudo apt install python3-pip && pip3 install Flask`).
- **Step 3:** Create a basic Flask app. Example:
  ```python
  from flask import Flask, render_template

  app = Flask(__name__)

  @app.route('/')
  def home():
      return render_template('index.html')

  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=5000)
  ```
- **Step 4:** Run the app (`python3 app.py`).

**Understanding ASGI and WSGI:**
- **WSGI (Web Server Gateway Interface):** Standard interface between web servers and Python web applications (used by Flask, Django).
- **ASGI (Asynchronous Server Gateway Interface):** Successor to WSGI, supports asynchronous capabilities (used by frameworks like FastAPI).
- **Why WSGI for Flask:** Flask is traditionally synchronous, and WSGI is well-supported and suitable for most Flask applications.
  
**Production Deployment:**
- **Why Production Deployment is Important:** In production, debug mode is disabled for security, and a robust server setup ensures high availability and performance.
- **Using Gunicorn:** A WSGI server for running Python applications in production. It handles multiple requests simultaneously, making it suitable for production environments.

### **4. Setting Up a Git Repository and Uploading Code**

**Uploading Code to GitHub:**
- **Step 1:** Initialize a Git repository (`git init`).
- **Step 2:** Create a `.gitignore` file to exclude unnecessary files like `__pycache__/`.
- **Step 3:** Add and commit your files (`git add . && git commit -m "Initial commit"`).
- **Step 4:** Create a repository on GitHub and push your code (`git remote add origin <repo_url> && git push -u origin master`).

**Setting Up SSH Keys on Ubuntu Server:**
- **Step 1:** Generate SSH keys on your local machine (`ssh-keygen`).
- **Step 2:** Copy the public key to your server (`ssh-copy-id user@server_ip`).
- **Step 3:** Clone your private GitHub repository using SSH (`git clone git@github.com:user/repo.git`).

### **5. Deployment of Flask App on Production**

**Install Nginx:**
- **What is Nginx:** A high-performance web server that also acts as a reverse proxy, load balancer, and HTTP cache.
- **Why Use Nginx:** It serves static content efficiently, manages multiple Flask worker processes, and handles HTTPS traffic.
- **Installation:** `sudo apt install nginx`
- **Configuration:** Modify `/etc/nginx/sites-available/default` to forward traffic from Nginx to your Flask app running on Gunicorn.

**Running Flask App:**
- **Step 1:** Install Gunicorn (`pip3 install gunicorn`).
- **Step 2:** Run your Flask app with Gunicorn (`gunicorn --bind 0.0.0.0:5000 app:app`).
- **Step 3:** Configure Nginx to forward requests to Gunicorn.

**Disable Debug Mode:**
- **Importance:** Running Flask in debug mode in production is a security risk. Use `app.run(debug=False)` or configure Gunicorn to run your app in production mode.

**Accessing Flask App:**
- **Step 1:** Access your app via the server’s public IP or domain name.
- **Step 2:** Verify that the application is running smoothly without debug logs.

### **6. Setting Up a Domain**

**Connecting a Domain:**
- **Step 1:** Register a domain on GoDaddy or another domain registrar.
- **Step 2:** Go to AWS Route 53 or your domain registrar's DNS settings and point the domain to your server’s static IP.

**Configuring Nginx for Domain:**
- **Step 1:** Update the Nginx server block in `/etc/nginx/sites-available/default` to listen to your domain name.
- **Step 2:** Restart Nginx to apply changes.

**Enabling SSL with Certbot:**
- **What is SSL:** Secure Sockets Layer (SSL) is a standard technology for keeping an internet connection secure and safeguarding any sensitive data that is being sent between two systems.
- **TLS Handshake:** The process by which a client and server establish a secure communication session. TLS is the successor to SSL.
- **Install Certbot:** `sudo apt install certbot python3-certbot-nginx`.
- **Generate SSL Certificate:** `sudo certbot --nginx -d yourdomain.com`.
- **Automatic Renewal:** Certbot sets up automatic renewal for SSL certificates.

**Accessing Flask App via HTTPS:**
- **Step 1:** Ensure your domain points to your server’s IP.
- **Step 2:** Access your app via `https://yourdomain.com`.


### **Creating a Simple HTML Index Page with Bootstrap 5 and Sample Configuration Files**

To complete your Flask application setup, we'll create a simple `index.html` file that uses Bootstrap 5 for styling and includes a navigation bar. Additionally, we'll provide sample `.gitignore` and `.env` files to help manage your project effectively.

#### **1. `templates/index.html`**

Flask uses the Jinja2 templating engine by default, and HTML templates are typically stored in a `templates` directory within your project. Here's a simple `index.html` file that includes a Bootstrap 5 navbar and displays the content "Deployment of Flask APP on AWS Cloud".

**Directory Structure:**
```
your_project/
├── app.py
├── requirements.txt
├── templates/
│   └── index.html
├── static/
│   └── (optional static files like CSS, JS, images)
├── .gitignore
└── .env
```

**`templates/index.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask App Deployment on AWS</title>
    <!-- Bootstrap 5 CSS CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <!-- Navigation Bar -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">Flask AWS Deployment</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" 
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <!-- Add more navbar items here if needed -->
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <!-- Example nav items -->
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#">Home</a>
                    </li>
                    <!-- Add more links as necessary -->
                </ul>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <div class="container mt-5">
        <h1 class="text-center">Deployment of Flask APP on AWS Cloud</h1>
        <p class="text-center">This is a simple Flask application deployed on AWS using LightSail.</p>
    </div>

    <!-- Bootstrap 5 JS and dependencies CDN -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**Explanation:**
- **Bootstrap 5 CDN:** The `<link>` tag in the `<head>` includes the Bootstrap 5 CSS from a CDN for styling. Similarly, the `<script>` tag before the closing `</body>` tag includes Bootstrap's JavaScript bundle.
- **Navbar:** Utilizes Bootstrap's navbar component with a brand name and a collapsible menu for responsiveness.
- **Main Content:** Displays a centered heading and a paragraph describing the application.

#### **2. `.gitignore` File**

A `.gitignore` file specifies intentionally untracked files that Git should ignore. This is essential to prevent sensitive information, compiled files, and dependencies from being committed to your repository.

**Sample `.gitignore`:**
```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Virtual environment
venv/
ENV/
env/
.venv/
.env/

# Distribution / packaging
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
*.egg-info/
.installed.cfg
*.egg

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# PyCharm
.idea/

# VS Code
.vscode/

# Other files to ignore
.DS_Store
Thumbs.db
```

**Explanation:**
- **Virtual Environments:** Folders like `venv/`, `ENV/`, and `.venv/` are commonly used for Python virtual environments and should be excluded.
- **Compiled Files:** Directories like `__pycache__/` and file patterns like `*.pyc` are generated by Python and don't need to be tracked.
- **Environment Variables:** Files like `.env` often contain sensitive information and should be ignored.
- **IDE Specific:** Directories like `.idea/` for PyCharm and `.vscode/` for VS Code store IDE-specific settings and should not be part of the repository.
- **Others:** Various other patterns to ignore compiled files, logs, test reports, etc.

#### **3. `.env` File**

A `.env` file is used to store environment variables, such as configuration settings and secrets, which should not be hard-coded into your application. This file is typically excluded from version control using `.gitignore`.

**Sample `.env`:**
```
# Flask Configuration
FLASK_ENV=production
FLASK_DEBUG=False
SECRET_KEY=your_secret_key_here

# Database Configuration (if applicable)
DATABASE_URL=postgresql://username:password@localhost:5432/yourdatabase

# AWS Credentials (if needed, though it's better to use IAM roles)
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key

# Other Configuration Variables
# For example:
# API_KEY=your_api_key
# OTHER_SETTING=your_setting
```

**Explanation:**
- **FLASK_ENV:** Specifies the environment in which Flask is running (`development`, `production`, etc.). Setting it to `production` ensures that debug mode is off and optimizations are enabled.
- **FLASK_DEBUG:** Explicitly disables debug mode. When set to `False`, Flask will not display detailed error pages, which is important for security in a production environment.
- **SECRET_KEY:** A secret key used by Flask to secure sessions and other security-related needs. **Important:** Replace `your_secret_key_here` with a strong, random key. Keep this value confidential.
- **DATABASE_URL:** If your application uses a database, this variable can store the connection string. Replace the placeholder with your actual database credentials and information.
- **AWS Credentials:** If your application interacts with AWS services directly and requires credentials, you can store them here. **Note:** For enhanced security, it's recommended to use IAM roles attached to your EC2 or LightSail instance instead of hard-coding credentials.
- **Other Configuration Variables:** You can add additional environment-specific settings as needed for your application.

**Usage in `app.py`:**

To utilize the variables defined in your `.env` file within your Flask application, you can use the `python-dotenv` package or manually load them. Here's an example using `python-dotenv`:

1. **Install `python-dotenv`:**
   ```bash
   pip install python-dotenv
   ```

2. **Modify `app.py` to load environment variables:**
   ```python
   from flask import Flask, render_template
   from dotenv import load_dotenv
   import os

   # Load environment variables from .env file
   load_dotenv()

   app = Flask(__name__)
   app.config['SECRET_KEY'] = os.getenv('SECRET_KEY')

   @app.route('/')
   def home():
       return render_template('index.html')

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

**Explanation:**
- **Loading Environment Variables:** The `load_dotenv()` function reads the `.env` file and loads the variables into the environment, making them accessible via `os.getenv()`.
- **Using `SECRET_KEY`:** The `SECRET_KEY` is retrieved from the environment and set in the Flask application's configuration.

**Security Best Practices:**
- **Never Commit `.env` to Version Control:** Ensure that your `.env` file is listed in `.gitignore` to prevent sensitive information from being exposed in your repository.
- **Use Strong Secrets:** Generate strong, random values for `SECRET_KEY` and other sensitive variables.
- **Limit AWS Credentials Exposure:** Prefer using IAM roles for AWS services over embedding credentials in `.env` files.
