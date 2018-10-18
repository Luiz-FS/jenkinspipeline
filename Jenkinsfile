pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "cp -r **/target/*.war /home/ecis/apache-tomcat-8.5.34-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp -r **/target/*.war /home/ecis/apache-tomcat-8.5.34-prod/webapps"
                    }
                }
            }
        }
    }
}
