pipeline {
    agent any
    environment {
        MAVEN_HOME = '/usr/local/apache-maven'
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/BibhuAnand/ANAND1005.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build('your-image-name', '-f Dockerfile .')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://your-registry-url', 'docker-credentials-id') {
                        def app = docker.build('your-image-name')
                        app.push('latest')
                    }
                }
            }
        }

        stage('Deploy to AWS ECS') {
            steps {
                script {
                    sh 'aws ecs update-service --cluster your-cluster --service your-service --force-new-deployment'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and deployment succeeded!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
