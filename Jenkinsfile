pipeline {
    agent any
    echo 'Entry Point 1 ... confirming Maven works'
    tools {
        maven 'localMaven'
    }
    stages{
        stage('Build'){
            echo 'Entry Point 2 ... build start'
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}