pipeline {

  agent { label 'Linux-server'}

  environment {
   DOCKERHUB_CREDENTIALS=credentials('dockerhub-devopseje')
  }

  stages {

     stage('git clone project'){

         steps {
          git 'https://github.com/devopseje/nodeapp_test.git'
         }
     }

     stage('Build'){
        steps {
         sh 'docker build -t devopseje/nodeapp_test:latest .'
        }
     }

     stage('Login into Docker'){
        steps{
           sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS --password-stdin'
        }
     }

     stage('Docker Push into Docker-Hub'){
       steps{
        sh 'docker push devopseje/nodeapp_test:latest'
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