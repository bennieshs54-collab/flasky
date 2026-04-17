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

<img width="925" height="470" alt="image" src="https://github.com/user-attachments/assets/865d161f-1565-4cf4-b041-070bffae7556" />

<img width="940" height="350" alt="image" src="https://github.com/user-attachments/assets/8fba2263-5721-4570-b186-15019e81a2bf" />

<img width="940" height="481" alt="image" src="https://github.com/user-attachments/assets/452d02fc-2fca-4e93-bb9b-68066bcb468a" />

* Python3 and pip configured

<img width="940" height="275" alt="image" src="https://github.com/user-attachments/assets/ea12d54e-335f-48e9-a3d0-4d4e038d3a57" />


* Required Jenkins plugins installed:

<img width="940" height="272" alt="image" src="https://github.com/user-attachments/assets/dd197d5c-8753-43e1-913c-684e7d95e559" />


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



<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/2eb5555b-0662-4bb8-85c7-c211d2901327" />

<img width="940" height="503" alt="image" src="https://github.com/user-attachments/assets/d26abbf0-5783-4852-b281-959a0a854d3f" />


<img width="940" height="83" alt="image" src="https://github.com/user-attachments/assets/cca23c8f-f6db-4411-b12d-721e761f1218" />

<img width="940" height="481" alt="image" src="https://github.com/user-attachments/assets/7dd79c56-bca5-412b-bba3-d017b0c24c34" />





---

## 🙌 Author

* Name: Benniesh.S
* Course: DevOps / CI-CD Assignment
* Platform: Herovired

---

