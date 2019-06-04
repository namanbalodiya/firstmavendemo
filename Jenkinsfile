pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.212.72.170', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.234.211.176', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '/home/user/Documents/*war'
                }
            }
        }
    }
}
