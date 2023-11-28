pipeline{
    agent any
     environment {
        PATH= "/opt/maven3/bin:$PATH"
    }
    stages {
        stage('Checkout Code') {
           steps {
              checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/priyavamsi/jenkinscicd']]])
           }
        }
        stage('SCM') {
           steps {
              git branch: 'main', credentialsId: 'success', url: 'https://github.com/priyavamsi/jenkinscicd'
            }
        }
        stage('Maven Build') {
           steps {
              sh "mvn clean package"
              sh "mv target/*.jar target/myweb.jar"
            }
        }
        stage('deploy-dev') {
           steps{
               script {
                   withCredentials([sshUserPrivateKey(credentialsId: 'tomcat-new', keyFileVariable: 'KEYFILE')]) {
              sshagent(['tomcat-new']) {
              sh """
              scp -o StrictHostKeyChecking=no target/myweb.jar ec2-user@172.31.4.216:/home/ec2-user/apache-tomcat-9.0.83/webapps/
              ssh ec2-user@172.31.4.216 "/home/ec2-user/apache-tomcat-9.0.83/bin/shutdown.sh"
              ssh ec2-user@172.31.4.216 "/home/ec2-user/apache-tomcat-9.0.83/bin/startup.sh"
              """
            }
         }
       }
     }
   }
 }
}
