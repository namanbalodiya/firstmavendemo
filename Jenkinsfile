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
                    archiveArtifacts artifacts: '/var/lib/jenkins/jobs/fullyautomated/builds/11/archive/target/firstdemoproject-1.0-SNAPSHOT.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/jenkins/pipelinetest.pem /var/lib/jenkins/jobs/fullyautomated/builds/11/archive/target/firstdemoproject-1.0-SNAPSHOT.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/pipelinetest.pem /var/lib/jenkins/jobs/fullyautomated/builds/11/archive/target/firstdemoproject-1.0-SNAPSHOT.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
