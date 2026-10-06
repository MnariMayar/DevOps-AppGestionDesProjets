pipeline {

    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                git 'https://github.com/MnariMayar/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Build') {
            steps {
                dir('backend') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage('Tests') {
            steps {
                dir('backend') {
                    sh 'mvn test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('backend') {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            mvn sonar:sonar \
                            -Dsonar.projectKey=DevOps-AppGestionDesProjets \
                            -Dsonar.projectName="DevOps-AppGestionDesProjets"
                        '''
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t projet-backend:latest ./backend'
                sh 'docker build -t projet-frontend:latest ./frontend'
            }
        }

        stage('Docker Compose') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Containers') {
            steps {
                sh 'docker compose ps'
            }
        }
    }
}
