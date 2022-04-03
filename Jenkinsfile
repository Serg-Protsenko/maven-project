pipeline {
  agent any
  triggers {
    pollSCM('*/5 * * * *')
  }
  stages{
       stage ('Build'){
        steps {
          sh 'mvn clean package'
        }
         post {
         success {
           echo 'Archving...'
           archiveArtifacts artifacts:'**/target/*.war'
         }
        }
       }
           stage ('Deployments') {
             parallel{
               stage ('Deploy to staging')
                 steps {
                   sh "cp **/target/*.war /home/guy/programms/tomcat-staging/webapps"
                 }     
               }
               stage ('Deploy to prod') {
                 steps {
                   timeout(time:5, unit:'DAYS'){
                   input message:'Approve Prod deployment?'
                 }
                 sh "cp **/target/*.war /home/guy/programms/tomcat-prod/webapps"od'
               }
             }     
           }
         }
       }
