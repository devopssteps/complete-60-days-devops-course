# day 08- Git & GitHub Basics

### 📌 What is Git & GitHub?
| Concept | Description                                             |
| ------- | ------------------------------------------------------- |
| Git     | Version control system to track code changes locally    |
| GitHub  | Cloud platform to host Git repositories and collaborate |

### 🛠️ Install Git
 - 🔹 Windows: Download from https://git-scm.com
 - 🔹 Linux:
```sh
sudo apt update
sudo apt install git -y
```
Check version:
```sh
git --version
```
Set config:
```sh
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### 📁 Create Local Git Repository
```sh
mkdir demo-project
cd demo-project
git init
```
🎙️ “This creates a .git folder which tracks all changes in this directory.”

### 📝 Add a File & Commit
```sh
echo "Hello Git" > index.txt
git status
git add index.txt
git commit -m "Initial commit"
```
🎙️ “We staged the file using git add and saved the version with git commit.”

### 🌳 Branching and Merging
```sh
git branch feature-1
git checkout feature-1
echo "New Feature" >> index.txt
git add .
git commit -m "Add feature"
git checkout main
git merge feature-1
```
🎙️ “This is how teams work on separate features and later merge them into the main code.”

### ☁️ Push to GitHub
 - Step 1: Create a new repo on https://github.com
 - Step 2: Link remote & push
```sh
git remote add origin https://github.com/your-username/demo-project.git
git branch -M main
git push -u origin main
```
🎙️ “Now your local project is hosted in GitHub and can be shared or collaborated.”


### 👯 Clone a Repository
```sh
git clone https://github.com/your-username/demo-project.git
```

### 🔄 Pull Latest Changes
```sh
git pull origin main
```
🎙️ “Always pull changes before pushing new code to avoid conflicts.”

### 📌 Summary
 - ✅ Git is your local version tracker
 - ✅ GitHub is your online code host
 - ✅ You now know ```init```, ```add```, ```commit```, ```push```, ```pull```, ```branch```, and ```merge```






