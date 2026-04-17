# Flask CI/CD Pipeline Project (Jenkins + GitHub Actions)

##  Objective

The objective of this project is to implement a **CI/CD pipeline** for a Python Flask web application using:

* Jenkins (Continuous Integration & Deployment)
* GitHub Actions (Automated CI/CD workflows)

This project demonstrates how modern DevOps practices automate building, testing, and deploying applications efficiently.

---

## Tech Stack

* Python 3
* Flask
* Pytest
* Jenkins
* GitHub Actions
* GitHub

---

## Project Structure

```
flask-ci-cd-project/
│
├── app.py                  # Flask application
├── requirements.txt        # Python dependencies
├── test_app.py             # Unit tests
├── Jenkinsfile             # Jenkins pipeline definition
├── README.md               # Project documentation
│
└── .github/
    └── workflows/
        └── ci-cd.yml       # GitHub Actions workflow
```

---

## Application Overview

A simple Flask web application that returns:

```
Hello CI/CD!
```

This application is used to demonstrate automated CI/CD pipelines.

---

#  PART 1: Jenkins CI/CD Pipeline

##  Setup

* Jenkins installed on a CentOS Virtual Machine (Oracle VirtualBox)
* Java (OpenJDK 17) installed
* Python3 and pip configured
* Required Jenkins plugins installed:

  * Git Plugin
  * Pipeline Plugin

---

##  Pipeline Configuration

The Jenkins pipeline is configured using:

```
Pipeline script from SCM
```

This means Jenkins reads the pipeline configuration from the **Jenkinsfile stored in the GitHub repository**.

---

## Jenkinsfile Stages

###  Build Stage

* Installs dependencies from `requirements.txt`

###  Test Stage

* Runs unit tests using `pytest`

###  Deploy Stage

* Runs the Flask application

---

##  Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        PATH = "/var/lib/jenkins/.local/bin:${env.PATH}"
    }

    stages {

        stage('Build') {
            steps {
                sh 'pip3 install --user -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'nohup python3 app.py &'
            }
        }
    }
}
```

---

##  Trigger

* Configured using **GitHub Webhooks**
* Pipeline runs automatically on push to the repository

---

##  Notifications

* Email notifications configured for:

  * Build Success
  * Build Failure

---

##  Jenkins Output

The pipeline execution includes:

* Successful Build stage
* Successful Test stage
* Successful Deployment

(Screenshots attached separately as per submission requirements)

---

#  PART 2: GitHub Actions CI/CD Pipeline

##  Workflow Setup

* Workflow file located at:

```
.github/workflows/ci-cd.yml
```

---

##  Workflow Triggers

* Push to `main` branch → Run CI pipeline
* Push to `staging` branch → Deploy to staging
* Release creation → Deploy to production

---

## Workflow Configuration

```yaml
name: Flask CI/CD Pipeline

on:
  push:
    branches:
      - main
      - staging
  release:
    types: [created]

jobs:

  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: 3.10

    - name: Install Dependencies
      run: pip install -r requirements.txt

    - name: Run Tests
      run: python3 -m pytest

  deploy-staging:
    if: github.ref == 'refs/heads/staging'
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Deploy to Staging
      run: echo "Deploying to staging..."

  deploy-production:
    if: github.event_name == 'release'
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Deploy to Production
      run: echo "Deploying to production..."
```

---

## Secrets Configuration

GitHub Secrets used for secure deployments:

* `API_KEY`
* `DEPLOY_KEY`

---

##  How to Run Locally

```bash
pip install -r requirements.txt
python app.py
```

Open in browser:

```
http://localhost:5000
```

---

##  Testing

Run tests locally:

```bash
python3 -m pytest
```

---

##  Screenshots (Included in Submission)

* Jenkins pipeline stages (Build, Test, Deploy)
* Jenkins console output
* GitHub Actions workflow execution

---

##  Challenges Faced

* Jenkins could not detect `pytest` due to PATH issues
* Fixed by updating environment PATH in Jenkinsfile
* Initial project was complex → simplified to basic Flask app
* Dependency errors resolved by minimizing requirements

---

##  Conclusion

Successfully implemented:

✔ Jenkins CI/CD pipeline
✔ GitHub Actions workflow
✔ Automated build, test, and deployment
✔ Webhook-based triggers

This project demonstrates a complete DevOps pipeline for a Python Flask application.


---

## 🙌 Author

* Name: Benniesh.S
* Course: DevOps / CI-CD Assignment
* Platform: Herovired

---

