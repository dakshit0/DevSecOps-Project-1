pipeline {
    agent any
    tools {
        maven 'maven-server' 
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
	stage ('Build & JUnit Test') {
	steps {
		sh 'mvn install' 
	}
	post {
	    success {
		   junit 'target/surefire-reports/**/*.xml'
	    } 
	}
        }
        
    }
}
