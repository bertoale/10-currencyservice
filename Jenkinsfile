pipeline{
  agent any

  environment {
    DOCKERHUB_USER = 'albertxp'
    IMAGE_PREFIX = "${DOCKERHUB_USER}/10"
    REGISTRY_CREDENTIALS = 'dockerhub-creds'
  }

  stages {
    stage('Bersihkan Workspace'){
      steps{
        deleteDir()
      }
    }
    stage("Checkout Projek") {
      steps{
        git url: 'https://github.com/bertoale/10-currencyservice.git', branch: 'main'
      }
    }
    stage("Build Image currencyservice"){
      steps{
        sh "docker build -t ${IMAGE_PREFIX}-currencyservice:latest ."
      }
    }
    stage("Push Image currencyservice ke Dokcerhub (Production)"){
      steps{
        script{
          docker.withRegistry("https://index.docker.io/v1/", REGISTRY_CREDENTIALS){
          sh "docker push ${IMAGE_PREFIX}-currencyservice:latest"
          }
        }
      }
    }
    stage("Deploy ke Production"){
      steps{
        sh "kubectl apply -f currencyservice.yaml"
      }
    }
  }
  post {
        always {
            cleanWs()
        }
    }

}