pipeline{
  agent{
    docker {
      image 'node'
    }
  }

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