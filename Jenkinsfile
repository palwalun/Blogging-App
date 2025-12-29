pipeline {
 agent any
 parameters { 
  choice (
    name: 'ENV', choices: ['Dev', 'QA', 'PROD'], description: 'Select environment'
     )
	}
  stages{
   stage('Checkout'){
    steps{
	 checkout scm
	}
   }
   stage('Build'){
    steps{
	  sh 'mvn clean package'
	}
   }
   stage('Test'){
    steps{
	 script{
	  try{
	   sh 'mvn clean test'
	  }
	  catch (Exception e){
	   echo "Logs of failure: ${e}"
	  }
	 }
	 }
	 }
	 stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv('SonarQube') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BankApp \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=BankApp '''
    
                }
            }
        }
	 stage('Build docker image'){
      steps{
	   script{
	    try{
		 sh 'docker build -t blogging:latest'
		}
		catch (Exception e){
		 echo "Logs: ${e}"
		}
	   }
	  }
     }
  }
    post{
	 always{
	  cleanWs()
	 }
	 failure{
	  mail to: 'walunjpallavi69@gmail.com',
	        subject: 'Build Failed',
			body: 'Check jenkins console logs'
	 }
	}
}  