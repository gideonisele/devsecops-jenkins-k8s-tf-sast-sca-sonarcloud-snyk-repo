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
    stage('Build') {
	    steps {
		    sh 'mvn install'
	    }
    }
    stage('Test') {
	    steps {
		    sh 'mvn test'
	    }
    }
        stage('SCA scan') {
            steps {
                // Run Snyk SCA scan using the stored token
                sh '''
                snyk auth $SNYK_TOKEN
                snyk test --severity-threshold=high --all-projects
                '''
            }
    }
      post {
        always{
            junit '**/target/surefire-reports/*.xml'                
        }
        failure {
            echo 'Build failed due to vulnerabilities in thirdparty libraries'
        }
  }
}
}
