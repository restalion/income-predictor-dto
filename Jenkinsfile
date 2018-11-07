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
                echo "no tests in DTOs project"
            }
        }

        stage('Mutation tests') {
            steps {
                echo "-=- execute mutation tests -=-"
                echo "no tests in DTOs project"
            }
        }

        stage('Package') {
            steps {
                echo "-=- packaging project -=-"
                sh "mvn package -DskipTests"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Build Docker image') {
            steps {
                echo "-=- build Docker image -=-"
                echo "Not an executable project so no Docker image needed"
            }
        }

        stage('Run Docker image') {
            steps {
                echo "-=- run Docker image -=-"
                echo "Not an executable project so no Docker image run needed"
            }
        }

        stage('Integration tests') {
            steps {
                echo "-=- execute integration tests -=-"
                echo "Not an executable project so no integration test phase needed"
            }
        }

        stage('Performance tests') {
            steps {
                echo "-=- execute performance tests -=-"
                echo "Not an executable project so no performance test phase needed"
            }
        }

        stage('Dependency vulnerability tests') {
            steps {
                echo "-=- run dependency vulnerability tests -=-"
                sh "mvn dependency-check:check"
                dependencyCheckPublisher failedTotalHigh: '30', unstableTotalHigh: '25', failedTotalNormal: '110', unstableTotalNormal: '100'
            }
        }

        stage('Code inspection & quality gate') {
            steps {
                echo "-=- run code inspection & check quality gate -=-"
                withSonarQubeEnv('ci-sonarqube') {
                    sh "mvn sonar:sonar"
                }
                timeout(time: 10, unit: 'MINUTES') {
                    //waitForQualityGate abortPipeline: true
                    script  {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK' && qg.status != 'WARN') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Push Docker image') {
            steps {
                echo "-=- push Docker image -=-"
                echo "Not an executable project so no Docker image needed"
            }
        }
    }

    post {
        always {
            echo "-=- remove deployment -=-"
            echo "Not an executable project so no Docker image needed"
        }
    }
}