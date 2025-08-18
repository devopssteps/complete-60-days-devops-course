## Day-18: Jenkins with Email Notifications

### 🔧 Objective:
 - Send email notifications on build success/failure
 
### ✅ Jenkins Email Notification
🔧 Prerequisites:
 - Jenkins installed
 - Jenkins server has internet access
 - SMTP server (e.g., Gmail SMTP, AWS SES)

### 🔨 Step-by-Step: Configure Email Notification
🧱 1. Install Email Extension Plugin
  - Go to Manage Jenkins → Manage Plugins
  - Search for: Email Extension Plugin
  - Install and restart Jenkins

🧱 2. Configure SMTP Server
  - Go to Manage Jenkins → Configure System
  - Scroll to Extended E-mail Notification
```sh
SMTP server: smtp.gmail.com  
Use SSL: ✔  
SMTP Port: 465  
SMTP Username: your-email@gmail.com  
SMTP Password: App password from Gmail  
```
⚠️ For Gmail, enable App Passwords from your Google account.

🧱 3. Add Default Email Settings
```sh
Default user e-mail suffix: @gmail.com  
Default Recipients: your-email@gmail.com  
```

🧱 4. Add Email Step in Pipeline
```sh
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        success {
            emailext subject: 'Build SUCCESS: ${JOB_NAME}',
                     body: 'Job ${JOB_NAME} build #${BUILD_NUMBER} was successful.',
                     to: 'your-email@gmail.com'
        }
        failure {
            emailext subject: 'Build FAILURE: ${JOB_NAME}',
                     body: 'Job ${JOB_NAME} build #${BUILD_NUMBER} failed.',
                     to: 'your-email@gmail.com'
        }
    }
}
```
✅ Test by running the pipeline. You'll get an email on success or failure.


