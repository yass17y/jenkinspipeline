pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.154.241.222', description: 'Staging Server')
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
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
			sh "scp -i /tmp/tomcat.pem **/target/*.war ec2-user@54.154.241.222:/opt/tomcat/webapps"
                    }
                }

            }
        }
    }
}
