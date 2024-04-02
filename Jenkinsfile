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
        sh "~/apache-tomcat-7.0.94/bin/startup.sh"
      }
    post {
      always {
        // Send email notification on completion
        emailext (
             subject: "Jenkins Build ${currentBuild.currentResult} Jenkins CI-CD",
             body: "Check console output at $BUILD_URL",
             to: "towehcorina@gmail.com",
             mimeType: 'text/html'
          )
      }
    }
  } 
  }
