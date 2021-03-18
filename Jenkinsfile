pipeline{
  agent any

  environment{
    DOCKER_TAG = getDockerTag()
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
    stage('Buld and test image'){
      steps{
        echo "build docker images"
        script{
          api = docker.build("hisbu/qlass-api")
        }
        echo "run container for test image"
        sh "docker run -d --rm --name testapi -p 8081:2017 hisbu/qlass-api"
        input message: "Finished test image? (Click procced to continue)"

        echo "cleanup container testapi"
        sh "docker stop testapi"
      }
    }
    stage('Push image'){
      steps{
        script{
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-hisbu'){
            api.push("${DOCKER_TAG}")
            api.push("latest")
          }
        }
        echo "Cleanup image"
        sh 'docker rmi hisbu/qlass-api'
      }
    }
    stage("deploy"){
      steps{
        sh "chmod +x changeTag.sh"
        sh "./changeTag.sh ${DOCKER_TAG}"
        withKubeConfig([credentialsId: 'kubeconfig-clusterjcde', serverUrl: 'https://34.101.246.253']){
          sh 'kubectl apply -f deployment-config.k8s.yaml'
        }
      }
    }
  }
}

def getDockerTag(){
  def tag = sh script: "git rev-parse HEAD", returnStdout: true
  return tag
}