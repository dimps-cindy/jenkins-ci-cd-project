pipeline {
    agent {
        label 'build-server'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application'
                // Define build steps here
                sh '/opt/maven/bin/mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests'
                // Define test steps here
                sh 'mvn test'
                stash (name: 'Jenkins CI-CD', includes: "target/*.war")
            }
        }
        stage('Deploy') {
            agent {
                label 'deploy-server'
            }
            steps {
                echo 'Deploying the application'
                // Define deployment steps here
                unstash 'Jenkins CI-CD'
                sh "sudo rm -rf ~/apache*/webapps/*.war"
                sh "sudo mv target/*.war ~/apache*/webapps/"
                sh "sudo systemctl daemon-reload"
                sh "sudo ~/apache*/bin/shutdown.sh && sudo ~/apache*/bin/startup.sh"
            }
        }
    }
    post {
        success {
            mail to: "towehcorina@gmail.com",
            subject: "Jenkins CI-CD Successful",
            body: "Jenkins CI-CD was successfully built, tested, and deployed"
        }
        failure {
            mail to: "towehcorina@gmail.com",
            subject: "Jenkins CI-CD Failed",
            body: "Jenkins CI-CD failed, please investigate"
        }
    }
}
