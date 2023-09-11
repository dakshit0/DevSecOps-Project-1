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
