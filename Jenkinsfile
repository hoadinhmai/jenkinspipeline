pipeline {
    agent any
    tools{
        maven 'localMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.210.70.56', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.210.70.56', description: 'Production Server')
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
                        sh "scp -i /Users/hoa/Documents/dev/tomcatdemo.pem /Users/Shared/Jenkins/Home/workspace/fully-automated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/hoa/Documents/dev/tomcatdemo.pem /Users/Shared/Jenkins/Home/workspace/fully-automated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}