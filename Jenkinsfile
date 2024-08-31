pipeline {

    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/paint-it-green/WebGoat.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Authenticate Snyk') {
            steps {
                sh 'snyk auth e6ae76b9-8e9e-4110-ac0a-a83ef2410368'
            }
        }
        
        stage('Security  Scan') {
            steps {
                sh 'snyk test'
            }
        }

        stage('TruffleHog Secret Detection') {
            steps {
                sh 'trufflehog --json .'
            }
        }

        stage('Code Analysis with SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }
}
