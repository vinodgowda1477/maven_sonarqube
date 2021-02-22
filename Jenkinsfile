pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/vinodgowda1477/maven_sonarqube.git'
      }
    }

  stage('Build project') {
   steps {
	script {
    sh "mvn clean verify"
      }
    }
  }

   stage('Run SonarQube analysis') {
   steps { 
	script {   
  def scannerHome = tool 'SonarQubeScanner';
  withSonarQubeEnv('SonarQube') {
    sh "${scannerHome}/bin/sonar-scanner"
      }
    }
  }
}

    stage("Quality Gate") {
	steps{
	        script {
		      timeout(time: 1, unit: 'HOURS') {
		      def qg = waitForQualityGate()
		      if (qg.status != 'OK') {
			 mail bcc: '', body: "<b>build failed due to sonarqube qaulitygate failure.please check sonarqube for results</b><br> Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: ${env.JOB_NAME}", to: "vinod.r@analyttica.com";
			 error "piprline aborted due to quality gate failure: ${qg.status}"
          }
	}
     }
  }
}
}
  post {  
         success {  
             mail bcc: '', body: "<b>Pipeline completed successfully</b><br> Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "PIPELINE SUCCESSFUL-> CI: ${env.JOB_NAME} BUILD: ${env.BUILD_NUMBER}", to: "vinod.r@analyttica.com";
         }  
         failure {  
             mail bcc: '', body: "<b>build failed.please check jenkins build results</b><br> Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "PIPELINE ERROR-> CI: ${env.JOB_NAME} BUILD: ${env.BUILD_NUMBER}}", to: "vinod.r@analyttica.com";
         }  
     }  
}
