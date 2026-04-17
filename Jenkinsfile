pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/bennieshs54-collab/flasky.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo Building...'
            }
        }

        stage('Run') {
            steps {
                sh 'python3 app.py'
            }
        }
    }
}
