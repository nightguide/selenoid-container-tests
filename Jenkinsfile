#!groovy
  
pipeline {
  agent { label 'jenkins-slave' }
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
		  reuseNode true
        }
      }
      
      steps {
        //Send to Slack notify
        slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        //Build
        
  sh 'mvn clean test -Dgrid.connection.url=http://172.23.44.110:30012/wd/hub -Dgrid.browser.name=chrome -Dgrid.browser.version=69.0'
      }   
    }
    
  
     }
post {
    success {
      slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
    }

    failure {
      slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
    }     
 }
}

