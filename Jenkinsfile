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
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube-server') {
                        sh 'mvn clean verify sonar:sonar \
                           -Dsonar.projectKey=devsecops-1 \
                           -Dsonar.host.url=http://192.168.12.40:9000 \
                           -Dsonar.login=sqp_21f25097c11c0603ad1cbdff7303f1e7771fa70c'
                }
            }
        }
	stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }    
    }
}
