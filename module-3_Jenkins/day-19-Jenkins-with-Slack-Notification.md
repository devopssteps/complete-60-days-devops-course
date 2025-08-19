## Day-19: Jenkins with Slack Notifications
### 🔧 Objective:
  - Send Slack notifications from Jenkins pipelines

## ✅ Jenkins Slack Notification

### 🔧 Prerequisites:
 - Slack workspace
 - Jenkins Slack plugin

### 🔨 Step-by-Step: Configure Slack Notification

🧱 1. Install Slack Notification Plugin
   - Manage Jenkins → Manage Plugins
   - Install ```Slack Notification Plugin```
  
🧱 2. Configure Slack App
   - Go to your Slack Workspace
   - You already have an channel
   - Click your channel name at the top left
   - Go to Tools & Setting >> Manage apps
   - Type jenkins in the top search bar >> then select Jenkins CI
   - New page come >> Click Add to Slack
   - Choose and slect your channel name from the deop down list beide Post to Channel and the drop down list will take some time to come
   - After select your channel >> Now click Add Jenkins CI integration
   - Now one page open in this page all configuration and setps you get, Scroll down and steps 3 you get the following important inforation
      -A. Token : this is your token
      -B. Workspace : beside Team Subdomain
      -C. Channel name : Sroll down then you get the channel name beside Post to Channel (eg. #jenkins) 
   - With this above info you need, Now create a credentials with the above token

🧱 3. Create credentails in jenkins
   - Manage Jenkins → Credentials -> Scroll down click System -> Click global credentails -> Click Add credentials
       - Workspace : Put the workspace you get in above B.
       - Select secret text from kind
       - pest your token on in the secret
       - type any id and Description
       - Click Create
  
  
🧱 4. Configure Jenkins Slack
   - Manage Jenkins → Configure System / System -> Scroll down and go to slack section and put your data as below
   - Fill in:
      - Workspace : Put the workspace you get in above B.
      - Credentials : Slect the credentails you create the above steps
      - Type your channel name in Default channel (eg. #jenkins) make sure put # before your channel name
      - Click the ```Test Connection``` and make sure you get ```success```
      - click Save

    
🧱 5. Add Slack Step in Pipeline -demo-1
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
            slackSend(channel: 'C09AK2QM6M6', message: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
        failure {
            slackSend(channel: 'C09AK2QM6M6', message: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
    }
}
```
🧱 5. Add Slack Step in Pipeline -demo-2
```sh
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                // Simulating build step
                sh 'echo Hello World'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests..."
                // Simulating test step
                sh 'exit 1' // Change to 'exit 1' to test failure notification
            }
        }
    }

    post {
        success {
            slackSend(
                channel: 'C09AK2QM6M6', 
                message: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' completed successfully!\n${env.BUILD_URL}",
                color: '#36a64f'
            )
        }
        failure {
            slackSend(
                channel: 'C09AK2QM6M6', 
                message: "❌ FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed!\n${env.BUILD_URL}",
                color: '#ff0000'
            )
        }
    }
}
```

