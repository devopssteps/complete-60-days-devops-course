# Day-16: GitHub Webhook Integration

# 🧪 Hands-On Demo Steps: GitHub Webhook Integration in Jenkins
### 🔧 Prerequisites:
 - Jenkins installed and running (on local or server)
 - GitHub account and a public or private repo
 - Jenkins Git & GitHub plugins installed
 - Jenkins is accessible from GitHub (use ngrok for local or public IP for server)

### ✅ Step 1: Install Required Plugins in Jenkins
 - Go to Manage Jenkins → Plugins
 - Install:
     - Git Plugin
     - GitHub Plugin
     - GitHub Integration Plugin
 - Alreay have a gitgub repo with a Jenkinsfile
     - Jenkinsfile
     - READMME.md   

### ✅ Step 2: Create a Jenkins Job
 1. Go to Jenkins dashboard → New Item
 2. Enter name: github-webhook-job
 3. Select pipeline Project
 4. Under Source Code Management, choose Git
     - Paste your GitHub repo URL
     - If private, add credentials
 5. Under Build Triggers:
     -  ✅ Check: GitHub hook trigger for GITScm polling

### ✅ Step 3: Setup GitHub Webhook
 1. Go to your GitHub Repo → Settings → Webhooks
 2. Click Add Webhook
    - Payload URL:
      Use: http://<Jenkins-URL>/github-webhook/
      (e.g. if using ngrok: http://abc123.ngrok.io/github-webhook/)
    - Content type: application/json
    - Event: Just push or all events
    - ✅ Active → Click Add Webhook
   
 ### Optional - If you don't have real ip then use ngrok
   - Install ngrok and run & get publicly accessible URL
   ```sh
   wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
   tar -xvzf ngrok-v3-stable-linux-amd64.tgz
   rm ngrok-v3-stable-linux-amd64.tgz
   sudo cp ngrok /usr/bin

   ngrok	    # check rok is working

   # Now run the ngrok
   ngrok authtoken <get-the-token-from-your-ngrok-account>
   ngrok http 8080
```

### ✅ Step 4: Test Webhook
 1. Push a code change to GitHub:
```sh
echo "New change" >> README.md
git add .
git commit -m "Testing webhook"
git push origin main
```
 2. Watch Jenkins console — job will trigger automatically.

### 📌 Troubleshooting Tips:
 - Jenkins must be publicly reachable (use ngrok for local).
 - Webhook log in GitHub shows delivery status.
 - Ensure GitHub repo is linked correctly and Jenkins credentials are valid.    

 ### 🖼️ Suggested Thumbnail Text
🧲 "GitHub to Jenkins Webhook | Auto CI/CD Trigger!"



## not using this one
### ✅ Step 3: Add a Simple Build Step
In Build section, add Execute Shell and enter:
```sh
echo "Code pulled from GitHub!"
git log -1
```
