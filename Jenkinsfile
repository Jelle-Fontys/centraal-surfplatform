pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                Dir('/backend/centraal-surfplatform-backend') {
                    bat 'dotnet restore' 
                    bat 'dotnet build --no-restore' 
                }
            }
        }
    }
}

