pipeline {
    agent {
        label 'node1'
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
                // Correct the path to Maven
                sh '/opt/apache-maven-3.9.6/bin/mvn clean package'
            }
        }
        stage('Test') {
            agent {
                label 'node1'
            }
            steps {
                echo 'Running tests'
                // Define test steps here
                sh 'mvn test'
                stash (name: 'jenkins-ci-cd-project', includes: "target/*.war")
            }
        }
        stage('Deploy') {
            agent {
                label 'node2'
            }
            steps {
                echo 'Deploying the application'
                // Retrieve stashed artifacts
                unstash 'jenkins-ci-cd-project'
                sh "sudo rm -rf ~/apache*/webapps/*.war"
                sh "sudo mv target/*.war ~/apache*/webapps/"
                sh "sudo systemctl daemon-reload"
                sh "sudo ~/apache*/bin/shutdown.sh && sudo ~/apache*/bin/startup.sh"
            }
        }
    }
    post {
        success {
            mail to: "cynthianukege@gmail.com, sam883marc@gmail.com",
            subject: "jenkins-ci-cd-project Successful",
            body: "jenkins-ci-cd-project was successfully built, tested, and deployed"
        }
        failure {
            mail to: "cynthianukege@gmail.com, sam883marc@gmail.com",
            subject: "jenkins-ci-cd-project Failed",
            body: "jenkins-ci-cd-project failed, please investigate"
        }
    }
}
