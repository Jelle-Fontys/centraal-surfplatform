pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    bat 'dotnet restore' 
                    bat 'dotnet build --no-restore' 
                }
            }
        }
        stage('Test') {
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    bat 'dotnet test --no-build --no-restore --collect "XPlat Code Coverage"' 
                }
            }
            post {
                always {
                    recordCoverage(tools: [[parser: 'COBERTURA', pattern: '**/*.xml']], sourceDirectories: [[path: 'SimpleWebApi.Test/TestResults']])  
                }
            }
        }
        stage('Deliver') { 
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    bat 'dotnet publish centraal-surfplatform-backend --no-restore -o published'
                }
            }
            post {
                success {
                    archiveArtifacts 'published/*.*' 
                }
            }
        }
    }
}
