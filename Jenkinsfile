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
    stage('Buld and test image'){
      steps{
        echo "build docker images"
        script{
          api = docker.build("hisbu/qlass-api")
        }
        echo "run container for test image"
        sh "docker run -d --rum --name testapi -p 8081:2017 hisbu/qlass-api"
        input message: "Finished test image? (Click procced to continue)"
        
        echo "cleanup container testapi"
        sh "docker stop testapi"
      }
    }
  }
}