pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        label 'build-server'
      }
      steps {
        echo 'Building the application'
        //Define build steps here
        sh '/opt/maven/bin/mvn clean package'
      }
    }
    stage('Test') {
      agent {
        label 'build-server'
      }
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
        sh "sudo ~/apache-tomcat-7.0.94/bin/shutdown.sh && sudo ~/apache-tomcat-7.0.94/bin/startup.sh"
      }
    }
  }
    post {
        success {
          script {
            // If the pipeline runs successfully, notify stakeholders
            mail to: "towehcorina@gmail.com", 
                 subject: "Jenkins CI-CD Successful",
                 body: "Jenkins CI-CD was successfully build, tested and deployed"
              }
      failure {
        script {
        // If the pipeline fails, send a notification
          mail to: "towehcorina@gmail.com",
               subject: "Jenkins CI-CD Failed", 
               body: "Jenkins CI-CD failed, please investigate"
        }
