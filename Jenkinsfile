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
        script('Junit') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }
    post {
        success {
            echo 'success'
        }
        unsuccessful {
            echo 'failure'
        }
    }
        
}