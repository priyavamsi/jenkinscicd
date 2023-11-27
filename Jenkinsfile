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
              sh "mv target/*.jar target/myweb.jar*
            }
        }
        stage('deploy-dev') {
           steps{
              sshagent(['tomcat-new']) {
              sh """
              scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/success/target/devops-1.0-SNAPSHOT.jar ec2-user@172.31.15.77:/home/ec2-user/apache-tomcat-9.0.83/webapps/
              ssh ec2-user@172.31.15.77/home/ec2-user/apache-tomcat-9.0.83/bin/shutdown.sh
              ssh ec2-user@172.31.15.77/home/ec2-user/apache-tomcat-9.0.83/bin/startup.sh
              """
            }
        }
      }
    }
  }
