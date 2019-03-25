pipeline {
  agent {
    docker {
      image 'maven:3.5-jdk-8'
    }

  }
  stages {
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            sh '''mvn test
ls '''
          }
        }
        stage('Greeting') {
          steps {
            echo 'Hello'
          }
        }
      }
    }
    stage('Build') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Publish Test Results') {
      parallel {
        stage('Publish Test Results') {
          steps {
            junit(allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml')
          }
        }
        stage('Checkfiles') {
          steps {
            sh 'ls target/surefire-reports/*.xml'
          }
        }
      }
    }
    stage('Upload Artifects') {
      parallel {
        stage('Upload Artifects') {
          steps {
            archiveArtifacts(allowEmptyArchive: true, artifacts: 'target/**/*.jar')
          }
        }
        stage('Checkfiles') {
          steps {
            sh 'ls -R target'
          }
        }
      }
    }
  }
}