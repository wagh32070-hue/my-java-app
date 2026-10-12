pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3.9'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('3. Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('4. Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('5. Deploy') {
            steps {
                sh 'mkdir -p /tmp'
                sh 'cp target/my-java-app-1.0-SNAPSHOT.jar /tmp/deployed-app.jar'
                sh 'ls -lh /tmp/deployed-app.jar'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD Pipeline failed. Check the console output.'
        }
    }
}
