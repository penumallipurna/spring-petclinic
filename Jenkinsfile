pipeline {
    agent { label 'jdk11' }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    triggers {
         cron('0 * * * *')
    }
    stages {
        stage('Source code') {
            steps {
                git url: 'https://github.com/penumallipurna/spring-petclinic.git', 
                branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh script: 'mvn clean package'
            }
        }
        stage('Junit result') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }
    post {
        success {
            echo 'success'
            mail bcc: '', body: 'build success', cc: '', from: '', replyTo: '', 
            subject: 'build succeeded', to: 'purna.penumalli@gmail.com'
        }
        unsuccessful {
            echo 'failure'
            mail bcc: '', body: 'build success', cc: '', from: '', replyTo: '',
             subject: 'build succeeded', to: 'purna.penumalli@gmail.com'
        }
    }
        
}