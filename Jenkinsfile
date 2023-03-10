def githubUrl = "https://github.com/nemiro54/andersen-internship"
def tomcatServerUrl = "http://192.168.0.155:8080"
def tomcatCredentialsId = "978a0f7a-706c-4bf2-9575-68f2d6f47fd3"
def tomcatContextPath = "/andersen-internship"
pipeline {
  agent any
  tools {
    maven 'Maven 3.8.6'
  }
  triggers{
    pollSCM('* * * * *')
  }
  stages {
    stage ('Git') {
      steps{
        echo "Git step in process"
        git branch: 'main', url: "${githubUrl}"
        echo "Git step finished"
      }
    }
    stage ('Build') {
      steps {
        echo "Build in process"
        sh 'mvn clean package'
        echo "Build finished"
      }
    }
    stage ('Migration') {
      steps {
        echo "Database migration started"
        sh 'mvn flyway:migrate -Dflyway.configFiles=flyway.conf'
        echo "Database migration finished"
      }
    }
    stage ('Deploy') {
      steps {
        echo "Deploying is in process"
        script {
          deploy adapters: [tomcat9(credentialsId: "${tomcatCredentialsId}", path: '', url: "${tomcatServerUrl}")], contextPath: "${tomcatContextPath}", onFailure: false, war: 'target/*.war'
        }
        echo "Deploy finished"
      }
    }
  }
}