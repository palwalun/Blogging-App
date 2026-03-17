pipeline{
 agent any
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
  
 
 
 }


}