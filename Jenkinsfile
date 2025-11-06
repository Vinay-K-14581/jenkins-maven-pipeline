pipeline {
    agent any

    tools {
      maven 'maven 3.9.11'
    }
  environment {
        DEPLOY_DIR = '/tmp/deploy'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean compile -DskipTests=true'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Simulating deployment...'
                sh '''
                    mkdir -p ${DEPLOY_DIR}
                    cp target/*.jar ${DEPLOY_DIR}/
                    echo "Deployed artifact to ${DEPLOY_DIR}"
                '''
            }
        }
    }
  post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
