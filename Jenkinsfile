pipeline {
    agent any
    tools {
        maven 'maven-server' 
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
stage('Checkout git') {
     steps {
	git branch: 'main', url: 'https://github.com/dakshit0/DevSecOps-Project-1.git'
  }
}
