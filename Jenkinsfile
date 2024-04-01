pipeline {
  agent none
  strages {
    stage('Build') {
      steps {
        echo 'Building the application'
        //Define build steps here
        sh '/opt/maven/bin/mvn clean package
      }
    }
    stage ('Test') {
    steps {
      echo 'Running tests'
      //Define test steps here
      sh 'mvn test'
      stash (name: 'Jenkins CI-CD', includes: "target/*war")
    }
    }
    stage('Deploy') {
      agent {
        label 'deploy-server'
      }
      steps {
        echo 'Deploying the application'
        //Define deployment steps here
        unstash 'Jenkins CI-CD'
        sh "sudo rm -rf ~/apache*/webapp/*.war"
        sh "sudo mv target/*.war ~/apache*/webapps/"
        sh "sudo systemctl daemon-reload"
        sh "~/apache-tomcat-7.0.94/bin/startup.sh"
