pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                dir('backend/centraal-surfplatform-backend') {
                    bat 'dotnet restore' 
                    bat 'dotnet build --no-restore' 
                }
            }
        }
    }
}

