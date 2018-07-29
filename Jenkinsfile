pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.154.50.145', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.127.69.32', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "pscp -i D:\DEVOPS\Jenkins\workspace\PipelineasCodeEg28Jul2018\tomcat-demo.pem D:\DEVOPS\Jenkins\workspace\PipelineasCodeEg28Jul2018\webapp\target\*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -i D:\DEVOPS\Jenkins\workspace\PipelineasCodeEg28Jul2018\tomcat-demo.pem D:\DEVOPS\Jenkins\workspace\PipelineasCodeEg28Jul2018\webapp\target\*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}