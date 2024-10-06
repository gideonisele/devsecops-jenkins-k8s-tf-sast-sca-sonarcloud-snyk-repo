pipeline {
  agent any
  tools { 
        maven 'MAVEN3'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=new-app-java-sast -Dsonar.organization=new-app-java-sast -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=50d6aedeb92bc2f370ce794a7a60580bede0ea99'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		
  }
}
