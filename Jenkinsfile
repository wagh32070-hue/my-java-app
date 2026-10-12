
pipeline {
    agent any

    options {
        timeout(time: 15, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    tools {
        maven 'Maven-3.9'
        jdk 'JDK-17'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                echo 'Fetching source code from GitHub...'
                checkout scm
            }
        }

        stage('2. Build') {
            steps {
                echo 'Compiling the Java application...'
                sh 'mvn clean compile'
            }
        }

        stage('3. Unit Test') {
            steps {
                echo 'Running unit tests...'
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
                echo 'Packaging application into a JAR...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('5. Deploy') {
            steps {
                echo 'Deploying application artifact (local staging demo)...'
                sh '''
                    set -e
                    JAR_FILE=$(ls target/*.jar | grep -v '^target/original-' | head -n 1)
                    echo "Staging ${JAR_FILE} to /tmp/deployed-app.jar ..."
                    cp "${JAR_FILE}" /tmp/deployed-app.jar
                    ls -l /tmp/deployed-app.jar
                    echo "Local staging completed successfully."
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
