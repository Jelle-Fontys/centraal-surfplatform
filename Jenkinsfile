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
        // stage('Deliver') { 
        //     steps {
        //         dir('backend/centraal-surfplatform-backend') {
        //             sh 'dotnet publish ./API/API.csproj --no-restore -c Release -o ../published'
        //         }
        //     }
        //     post {
        //         success {
        //             archiveArtifacts artifacts: 'backend/published/**' 
        //         }
        //     }
        // }
    }
}
