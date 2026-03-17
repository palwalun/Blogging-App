pipeline{
 agent any
 environment {
		ACR_LOGIN_SERVER = "devopsproject1.azurecr.io"
		IMAGE_NAME = 'boardgame'
		TAG = 'latest'
    }
 parameters{
  choice (
    name: 'ENV', choices: ['Dev', 'Test', 'Prod'], description: 'Select Env'
  )
 }
 stages{
  stage('Checkout'){
   steps{checkout scm}
  }
  stage('Build'){
   steps{
   sh 'mvn clean package -DskipTests'
   }
  }
  stage('Build Docker Image'){
   steps{
   sh 'docker build -t boardgame .'
   }
  }
  stage('Login to ACR') {
       steps {
         withCredentials([usernamePassword(
             credentialsId: 'acr-creds',
             usernameVariable: 'ACR_USER',
             passwordVariable: 'ACR_PASS'
         )]) {
             sh '''
               echo $ACR_PASS | docker login $ACR_LOGIN_SERVER \
               -u $ACR_USER --password-stdin
             '''
           }
          }
         }
  stage('Tag Image') {
        steps {
         sh '''
           docker tag ${IMAGE_NAME}:${TAG} \
           $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}
         '''
          }
        }
  stage('Push Image to ACR'){
	     steps{
	      sh 'docker push $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}'
	    }
	   }
  stage('Build Docker Image'){
   steps{
   sh 'Kubectl apply -f deployment.yml'
   }
  }
 
 
 }
post{
 always{
  cleanWs()
 }

}

}