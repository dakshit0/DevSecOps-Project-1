pipeline {
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
    stage('Checkout git') {
     steps {
	git branch: 'main', url: 'https://github.com/praveensirvi1212/DevSecOps-Project-1'
  }
}
}
