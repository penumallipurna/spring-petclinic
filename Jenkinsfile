pipeline {
    agent { label 'jdk11' }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    triggers {
         cron('0 * * * *')
    }
    parameters {
        choice(name: 'GOAL', choices: ['compile', 'package', 'clean package'])
    }
    stages {
        stage('Source code') {
            steps {
                git url: 'https://github.com/penumallipurna/spring-petclinic.git', 
                branch: 'main'
            }
        }
        stage('Build and sonarqube-analysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST') {
                    sh script: "mvn ${params.GOAL}"
                }
                
                //stash name: 'spc-build', includes: 'target/*.jar'
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
            mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULT ${RUN_TESTS_DISPLAY_URL}", cc: '', from: '', replyTo: '', 
            subject: "${JOB_BASE_NAME} BUILD ${BUILD_ID} succeeded", to: 'purna.penumalli@gmail.com'
        }
        unsuccessful {
            echo 'failure'
            mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULT ${RUN_TESTS_DISPLAY_URL}", cc: '', from: '', replyTo: '',
             subject: "${JOB_BASE_NAME} BUILD ${BUILD_ID} failed", to: 'purna.penumalli@gmail.com'
        }
    }
        
}