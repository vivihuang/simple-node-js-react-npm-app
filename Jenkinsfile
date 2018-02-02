pipeline {
    agent {
        docker {
            image 'node:8-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
        FOO = "BAR"
    }
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'DEV', description: 'The target environment')
    }
    stages {
        stage('Build') {
            steps {
                sh 'node -v'
                sh 'npm prune'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('FOO') {
            steps {
                sh 'echo "sh FOO is $FOO"'
                echo "FOO is $FOO"
                echo "Hello ${params.DEPLOY_ENV}"
                sh 'echo "sh Hello ${params.DEPLOY_ENV}"'
            }
        }

        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                // input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
