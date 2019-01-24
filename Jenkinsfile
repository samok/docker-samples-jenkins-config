pipeline {
    agent none
    stages {
        stage('Test Workspace Jenkins') {
            agent any
            steps {
                echo "${env.WORKSPACE}"
                echo "${env.JENKINS_HOME}"
            }
        }
        stage('Test Python') {
            agent { docker 'python' }
            steps {
                sh 'python --version'
                sh 'python -c \'print("TESTING")\''
            }
        }
        stage('Test Install Angular') {
            agent { docker 'node:7-alpine' }
            steps {
                // Installer les dependances via npm en local et non en global
                // Cela cause des problemes de permission
                sh 'node --version'
                sh 'npm install @angular/cli'
            }
        }
    }
}