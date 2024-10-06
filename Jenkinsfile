pipeline {
  agent any
  environment {
	  SNYK_TOKEN = credentials('SNYK_TOKEN')
    }
  tools { 
        maven 'MAVEN3'  
    }
   stages {
       stage('Checkout') {
	    steps {
		    git branch: 'main', url: 'https://github.com/gideonisele/devsecops-jenkins-k8s-tf-sast-sca-sonarcloud-snyk-repo.git'
	    }
    }
    stage('Test') {
	    steps {
		    sh 'mvn test'
	    }
    }
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
    post {
        always {
            // Clean up workspace after the pipeline finishes
            cleanWs()
        }
        failure {
            echo 'Build failed due to vulnerabilities in thirdparty libraries'
        }
  }
}
