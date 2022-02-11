pipeline {

  agent { label 'Master'}

  environment {
   DOCKERHUB_CREDENTIALS=credentials('dockerhub')
   dockerimagename = "devopseje/nodeapp"
   dockerImage = ""
  }

  stages {

     stage('git clone project'){

         steps {
          git 'https://github.com/devopseje/nodeapp_test.git'
         }
     }

     stage('Build'){
        steps {
          script {
          dockerImage = docker.build dockerimagename
          }
        }
     }

     stage('Pushing the image into Dockerhub'){
       environment {
        registryCredentials = 'dockerhub'
       }
       steps{
         script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredentials){
           dockerImage.push("latest")
          }
         }
       }
     }

     stage('Deploing App to Kubernetes'){
        steps {
          script {
          withAWS(credentials: 'aws-cred', region: 'eu-central-1') {
            kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
          }
          }
        }
     }

     stage('Remove unused image'){
       steps{
         sh 'docker rmi devopseje/nodeapp_test:latest -f'
       }
     }
  }

  post {
     always{
      sh 'docker logout'
      }
  }

}