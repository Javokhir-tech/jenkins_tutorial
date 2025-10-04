pipeline {
    agent {
        docker {
            image 'maven:3.9.0-eclipse-temurin-17'
        }
    }
    stages {
        stage('Initialize') {
            steps {
                script {
                    currentBuild.displayName = "${env.JOB_NAME} - #${env.BUILD_NUMBER}"
                }
                echo "Build name is set to: ${currentBuild.displayName}"
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Running Unit Tests...'
                sh 'mvn test'
                junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Code Quality') {
            steps {
                echo 'Running Code Quality Analysis...'
                sh 'mvn sonar:sonar'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}