# Banking System - Daily Progress Report

## Date: March 27, 2025

## Overview
Today, we focused on managing Docker containers, troubleshooting MySQL service issues, and migrating the database from SQLite to MySQL for our Banking System project. The main goal was to ensure seamless database integration and stable application deployment.

## Tasks Completed

### 1. Docker Management
- **Building and Deploying Containers**
  - We used Docker Compose to build and start the required containers for our banking system.
    ```sh
    docker-compose build
    docker-compose up -d
    ```
- **Checking Running Containers**
  - Verified that the MySQL container was running properly:
    ```sh
    docker ps | grep mysql
    ```
- **Inspecting Application Logs**
  - Checked logs to debug issues in the banking system container:
    ```sh
    docker logs banking_system --follow
    ```
- **Accessing the Container Shell**
  - Connected to the running banking system container for manual checks:
    ```sh
    docker exec -it banking_system bash
    ```

### 2. Database Migration from SQLite to MySQL
- **Updating Django Settings**
  - Modified `settings.py` to use MySQL instead of SQLite:
    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'banking_db',
            'USER': 'root',
            'PASSWORD': 'rootpass',
            'HOST': 'host.docker.internal',
            'PORT': '3306',
        }
    }
    ```
- **Checking Database Connection**
  - Verified that MySQL was running and accessible:
    ```sh
    docker exec -it mysql_db mysql -u root -p -e "SHOW DATABASES;"
    ```
- **Running Migrations**
  - Applied Django migrations to set up the MySQL database schema:
    ```sh
    docker exec -it banking_system python manage.py migrate
    ```
- **Listing Migrations**
  - Checked if migrations were properly applied:
    ```sh
    docker exec -it banking_system python manage.py showmigrations
    ```

### 3. MySQL Service Management
- **Starting MySQL Manually**
  - Restarted MySQL service for troubleshooting:
    ```sh
    sudo systemctl start mysql
    ```
- **Checking MySQL Status**
  - Verified if MySQL was active and running:
    ```sh
    systemctl status mysql.service
    ```
- **Accessing the MySQL Database**
  - Logged into MySQL shell to inspect database records:
    ```sh
    mysql -u root -p
    ```
- **Checking Existing Tables**
  - Verified tables inside the MySQL database:
    ```sh
    USE banking_db;
    SHOW TABLES;
    ```

### 4. Cleanup and Optimization
- **Stopping and Removing Containers**
  - Shut down the banking system and database containers:
    ```sh
    docker stop banking_system mysql_db
    docker rm banking_system mysql_db
    ```
- **Pruning Unused Docker Resources**
  - Removed all stopped containers, networks, and images:
    ```sh
    docker system prune -a -f
    ```
- **Rebuilding Containers from Scratch**
  - Used the `--no-cache` option to ensure a fresh build:
    ```sh
    docker-compose build --no-cache
    docker-compose up -d
    ```

## Challenges Faced
- **Database Connection Issues**
  - Faced issues with MySQL service failing to start inside the container.
  - Resolved by restarting MySQL and checking logs for errors.
- **Permission Denied Errors**
  - Some MySQL operations required elevated privileges, so we had to adjust user permissions.
- **Docker Volume Persistence**
  - Ensured MySQL data was not lost by mounting persistent volumes.

## Next Steps
- Perform further testing to confirm successful migration.
- Optimize database queries for better performance.
- Implement additional security measures for database access.
- Automate backup solutions for database integrity.

## Notes
- Code and database configurations have been updated.
- Further discussion planned for tomorrow to finalize testing.

---
# Jenkins Pipeline for Banking System

## Overview
To automate our Banking System deployment, we implemented a **Jenkins pipeline** that pulls the latest code from GitHub, sets up the necessary environment, builds a Docker image, and deploys the application efficiently. This pipeline ensures smooth integration, continuous deployment, and minimizes manual intervention.

## Why Jenkins?
Jenkins is an open-source **automation server** widely used for **CI/CD (Continuous Integration and Continuous Deployment)**. It helps automate repetitive tasks like testing, building, and deploying applications, making it an essential tool for software development.

## Pipeline Workflow
### **1. Cloning the Repository**
Jenkins fetches the latest version of the code from GitHub. This step ensures that the deployment always uses the latest updates from the designated branch.

```groovy
stage('Clone Repository') {
    steps {
        echo "Cloning Repository..."
        git branch: 'Vishnu', 
            credentialsId: 'Git_Jenkins',
            url: 'https://github.com/sunny567s35/BankingSystem.git'
        echo "Repository Cloned Successfully!"
    }
}
```
- **Why?** Automating repository cloning avoids manual pulls and ensures consistency across deployments.
- **Key Element:** `git checkout` is used to switch to the correct branch.

### **2. Setting Up Python Environment**
Since our Banking System is a **Django-based application**, a Python environment is required.

```groovy
stage('Setup Python Environment') {
    steps {
        sh '''
            python3 -m venv venv
            source venv/bin/activate
        '''
        echo "Python Environment Setup Completed!"
    }
}
```
- **Why?** Isolates dependencies, preventing conflicts with other Python applications.
- **Key Element:** Virtual environments (`venv`) ensure a controlled package installation.

### **3. Logging into Docker Hub**
To use pre-built Docker images, Jenkins logs into **Docker Hub** using stored credentials.

```groovy
stage('Login to Docker Hub') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
            }
        }
        echo "Docker Login Successful!"
    }
}
```
- **Why?** Automates authentication for pulling and pushing images.
- **Key Element:** Uses Jenkins credentials (`withCredentials`) to avoid storing sensitive data in scripts.

### **4. Pulling and Running the Docker Image**
Instead of building the project locally, we pull a pre-configured Docker image from Docker Hub.

```groovy
stage('Build Docker Image') {
    steps {
        sh 'docker pull vishnuvardhandommeti/bankingsystem:khushi'
    }
}

stage('Deploy') {
    steps {
        sh 'docker run -d -p 8000:8000 --name banking-system vishnuvardhandommeti/bankingsystem:khushi'
    }
}
```
- **Why?** Pulling an image avoids rebuilding, reducing deployment time.
- **Key Element:** `docker run` deploys the application with proper port mappings.

## **Jenkins Pipeline Benefits**
- **Automated Workflow**: No manual steps required after code is pushed.
- **Consistency**: Ensures each deployment follows the same steps.
- **Faster Deployments**: Uses Docker for efficient containerized execution.

## **Final Thoughts**
By implementing this Jenkins pipeline, we have streamlined our Banking System’s deployment process. Future improvements could include integrating automated testing, monitoring, and better error handling mechanisms.

---

![Jenkins](https://github.com/user-attachments/assets/d4eb8986-6d43-40cd-a2a0-1cae7b3edaed)
![](https://github.com/user-attachments/assets/e7d5f07f-3d35-4dbc-8c3c-090accde3514)
![pipelines](https://github.com/user-attachments/assets/85fcf833-9a1a-4c39-96b1-d30bcc6bde75)

![](https://github.com/user-attachments/assets/8d40cc4e-c3a2-494f-b1b7-2b24d60a8eb7)

# Forgot Password Feature (Email-Based Password Reset)

This document provides a detailed explanation of the forgot password feature in the `BankingSystem` Django project. This feature allows users to reset their password via email.

---

## Overview
The forgot password system consists of four main steps:
1. **User submits email** - A reset link is sent to their registered email.
2. **Email confirmation** - The user sees a confirmation message after submitting the request.
3. **Password reset form** - The user sets a new password using the link received in the email.
4. **Password reset complete** - A success message is displayed after resetting the password.

## URLs Configuration

The following URLs handle different steps of the password reset process:

```python
from django.urls import path, reverse
from django.contrib.auth import views as auth_views
from .views import forgot_password

urlpatterns = [
    # Step 1: Request Password Reset (User submits email)
    path('forgot-password/', forgot_password, name='password_reset'),
    
    # Step 2: Password Reset Done (Email Sent Confirmation)
    path('forgot-password/done/', auth_views.PasswordResetDoneView.as_view(template_name='password_reset_done.html'), name='password_reset_done'),
    
    # Step 3: Password Reset Confirm (User enters new password)
    path('reset/<uidb64>/<token>/', forgot_password, name='password_reset_confirm'),
    
    # Step 4: Password Reset Complete (Success Message)
    path('reset/done/', auth_views.PasswordResetCompleteView.as_view(template_name='password_reset_complete.html'), name='password_reset_complete'),
]
```

---

## Views
The `forgot_password` view handles both email submission and password reset functionality.

```python
from django.shortcuts import render, redirect
from django.contrib.auth.models import User
from django.contrib.auth.tokens import default_token_generator
from django.utils.http import urlsafe_base64_encode, urlsafe_base64_decode
from django.utils.encoding import force_bytes
from django.core.mail import send_mail
from django.contrib import messages
from django.urls import reverse


def forgot_password(request, uidb64=None, token=None):
    if request.method == "POST" and "email" in request.POST:
        email = request.POST.get("email")
        try:
            user = User.objects.get(email=email)
            uid = urlsafe_base64_encode(force_bytes(user.pk))
            token = default_token_generator.make_token(user)
            reset_link = request.build_absolute_uri(reverse('password_reset_confirm', kwargs={'uidb64': uid, 'token': token}))
            
            # Send password reset email
            send_mail(
                subject="Password Reset Request",
                message=f"Click the link to reset your password: {reset_link}",
                from_email="noreply@example.com",
                recipient_list=[email],
            )
            messages.success(request, "A reset link has been sent to your email.")
            return redirect("password_reset_done")
        except User.DoesNotExist:
            messages.error(request, "Email not found.")

    if uidb64 and token:
        try:
            uid = urlsafe_base64_decode(uidb64).decode()
            user = User.objects.get(pk=uid)
            if default_token_generator.check_token(user, token):
                if request.method == "POST":
                    new_password = request.POST.get("new_password1")
                    confirm_password = request.POST.get("new_password2")
                    if new_password != confirm_password:
                        messages.error(request, "Passwords do not match.")
                        return render(request, "password_reset_confirm.html", {"uidb64": uidb64, "token": token})
                    user.set_password(new_password)
                    user.save()
                    messages.success(request, "Password reset successful!")
                    return redirect("password_reset_complete")
                return render(request, "password_reset_confirm.html", {"uidb64": uidb64, "token": token})
            else:
                messages.error(request, "Invalid or expired reset link.")
                return redirect("password_reset")
        except (User.DoesNotExist, ValueError, TypeError):
            messages.error(request, "Invalid reset attempt.")
            return redirect("password_reset")
    
    return render(request, "password_reset_form.html")
```

---

## HTML Templates

### 1. `password_reset_form.html`
This template renders the email submission form.

```html
<form method="post">
    {% csrf_token %}
    <label for="email">Enter your email:</label>
    <input type="email" name="email" required>
    <button type="submit">Reset Password</button>
</form>
```

### 2. `password_reset_done.html`
This template confirms that the reset email was sent.

```html
<p>A password reset link has been sent to your email.</p>
```

### 3. `password_reset_confirm.html`
This template allows the user to set a new password.

```html
<form method="post">
    {% csrf_token %}
    <label for="new_password1">New Password:</label>
    <input type="password" name="new_password1" required>
    
    <label for="new_password2">Confirm Password:</label>
    <input type="password" name="new_password2" required>
    
    <button type="submit">Submit</button>
</form>
```

### 4. `password_reset_complete.html`
This template confirms that the password was successfully reset.

```html
<p>Your password has been successfully reset. You may now <a href="/login">log in</a>.</p>
```

---

## Summary
1. The user enters their email to request a password reset.
2. A reset link is emailed to the user.
3. The user follows the link and enters a new password.
4. A success message is displayed upon completion.

This implementation uses Django's built-in token generator to securely verify the password reset request.


This document ensures that the password reset functionality is well-documented for future development and debugging.


