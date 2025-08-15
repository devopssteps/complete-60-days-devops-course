## Day-15: Use parameters, post, environmet variables in Declarative Pipeline with hands on example

### 📦 What is a Jenkins Declarative Pipeline? 
 - Introduced in Jenkins 2.x
 - Easier to read and write than scripted pipeline
 - Defined using pipeline {} block
 - Often used in Jenkinsfile stored in your Git repo
<br>

✅ Why Declarative?
  - More readable
  - Built-in syntax validation
  - Recommended for teams

### ✅ Simple Declarative pipeline
```sh
pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Hello World 111'
            }
        }
        stage('test') {
            steps {
                echo 'Hello World test222'
            }    
        }
    }
}
```

### environment variable
```sh
environment {
        IMAGE_NAME = "devopssteps/my-app-15"
    }
```
### Add environment variable in pipeline
```sh
pipeline {
    agent any
    environment {
        IMAGE_NAME = "devopssteps/my-app"
    }
    stages {
        stage('build') {
            steps {
                echo "${IMAGE_NAME}"
            }
        }
        stage('test') {
            steps {
                echo 'Hello World test222'
            }    
        }
    }
}
```

✅ Use parameters:
```sh
parameters {
    string(name: 'ENV', defaultValue: 'dev', description: 'Environment')
}
```
### Demo: parameters use if else conditions

```sh
pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'prod'], description: 'Select the environment to deploy')
    }

    stages {
        stage('Print Selected Environment') {
            steps {
                echo "Selected Environment: ${params.ENVIRONMENT}"
            }
        }

        stage('Conditional Execution') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        echo "Deploying to Development environment"
                        // Add dev deployment logic here
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "Deploying to Staging environment"
                        // Add staging deployment logic here
                    } else if (params.ENVIRONMENT == 'prod') {
                        echo "Deploying to Production environment"
                        // Add production deployment logic here
                    } else {
                        error("Invalid environment selected!")
                    }
                }
            }
        }
    }
}
```
### When condition use in jenkinsfile

```sh
pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag')
        booleanParam(name: 'PUSH_IMAGE', defaultValue: true, description: 'Push image to Docker Hub?')
    }
    environment {
        IMAGE_NAME = "devopssteps/my-app-15"
    }

    stages {
        stage('clone') {
            steps {
                echo 'clone code............'
                checkout scm
            }
        }
        stage('build imgae') {
            steps {
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"
                sh "docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} ."
            }
        }
        stage('push imgae') {
            when {
                expression { return params.PUSH_IMAGE }
            }
            steps {
                echo "Building Docker image with tag: ${params.IMAGE_TAG}"
            }
        }
    }
}

```


### ✅ Add post block for notification:
```sh
post {
    success {
        echo 'Pipeline succeeded!'
    }
    failure {
        echo 'Pipeline failed.'
    }
}
```
