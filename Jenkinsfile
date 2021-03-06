#!groovy

pipeline {
    agent {
        docker {
            image 'maven:3.5.4-jdk-8'
            args '--network ci --mount type=volume,source=ci-maven-home,target=/root/.m2'
        }
    }

    environment {
        ORG_NAME = "oscuroweb"
        APP_NAME = "income-predictor-dto"
        APP_CONTEXT_ROOT = "oscuroweb"
        TEST_CONTAINER_NAME = "ci-${APP_NAME}-${BUILD_NUMBER}"
    }

    stages {
        stage('Compile') {
            steps {
                echo "-=- compiling project -=-"
                sh "mvn clean compile"
            }
        }

        stage('Unit tests') {
            steps {
                echo "-=- execute unit tests -=-"
                sh "mvn test"
            }
        }

        stage('Package') {
            steps {
                echo "-=- packaging project -=-"
                sh "mvn package -DskipTests"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Dependency vulnerability tests') {
            steps {
                echo "-=- run dependency vulnerability tests -=-"
                sh "mvn dependency-check:check"
            }
        }

        stage('Push Artifact') {
            steps {
                echo "-=- push Artifact -=-"
                echo "Not an executable project so no Docker image needed, anyway jar file need to be installed"
                sh "mvn install"
            }
        }
    }
}