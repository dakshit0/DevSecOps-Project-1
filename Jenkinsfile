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
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('sonarqube-server') {
                        sh 'mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=devsecops-1 \
                        -Dsonar.host.url=http://192.168.12.40:9000 \
                        -Dsonar.login=sqp_d427cc8078d87d969a830e2652c3cdaf86d5f0b2'
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
