pipeline {
  agent {
    docker {
      image 'maven:3.5-jdk-8'
    }
    
  }
  stages {
    stage('Test') {
      steps {
        parallel(
          "Test": {
            sh '''mvn test
ls '''
            
          },
          "Greeting": {
            echo 'Hello'
            
          }
        )
      }
    }
    stage('Build') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Publish Test Results') {
      steps {
        parallel(
          "Publish Test Results": {
            junit(allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml')
            
          },
          "Checkfiles": {
            sh 'ls target/surefire-reports/*.xml'
            
          }
        )
      }
    }
    stage('Upload Artifects') {
      steps {
        parallel(
          "Upload Artifects": {
            archiveArtifacts(allowEmptyArchive: true, artifacts: 'target/**/*.jar')
            
          },
          "Checkfiles": {
            sh 'ls target/**/*.jar'
            
          }
        )
      }
    }
  }
}