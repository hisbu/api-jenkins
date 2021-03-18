pipeline{
  agent any

  stages{
    stage('install depencencies'){
      steps{
        sh "npm install"
      }
    }
    stage('unit testing'){
      steps{
        echo "run unit testing"
      }
    }
  }
}