pipeline {
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }
    stages {
        stage('maven-version') {
            steps {
                sh 'mvn --version'
            }
        }
	stage('Checkout git') {
     steps {
	git branch: 'main', url: 'https://github.com/dakshit0/DevSecOps-Project-1'
  }
}    
    }
}
