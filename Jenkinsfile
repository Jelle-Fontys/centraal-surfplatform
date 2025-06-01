pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    sh 'dotnet restore'
                    sh 'dotnet build --no-restore' 
                }
            }
        }
        stage('Test') {
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    sh 'dotnet test --no-build --no-restore --collect "XPlat Code Coverage"' 
                }
            }
            post {
                always {
                    recordCoverage(tools: [[parser: 'COBERTURA', pattern: '**/TestResults/**/coverage.cobertura.xml']])  
                }
            }
        }
        stage('Docker Build & Push') {
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    withCredentials([usernamePassword(credentialsId: '80446a4c-d12b-43bc-b327-0c4701d2f6cd', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        script {
                            def imageName = "$DOCKER_HUB_USERNAME/centraal-surfplatform:latest"
                            sh """
                                docker build -t ${imageName} .
                                echo "$DOCKER_HUB_PASSWORD" | docker login -u "$DOCKER_HUB_USERNAME" --password-stdin
                                docker push ${imageName}
                            """
                        }
                    }
                }
            }
        }
    }
}
