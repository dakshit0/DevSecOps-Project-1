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
	git branch: 'main', url: 'git@github.com:dakshit0/DevSecOps-Project-1.git'
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
	    withSonarQubeEnv('SonarQube-server') {
		sh 'mvn clean verify sonar:sonar \
		-Dsonar.projectKey=devsecops-project-1 \
		-Dsonar.host.url=$sonarurl \
		-Dsonar.login=$sonarlogin'
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
stage('Docker Build') {
      steps {
           sh 'docker build -t dakshit0/sprint-boot-app:v1.$BUILD_ID .'
           sh 'docker image tag dakshit0/sprint-boot-app:v1.$BUILD_ID dakshit0/sprint-boot-app:latest'
	}
}
stage('Image Scan') {
	steps {
	sh ' trivy image --format template --template "@/usr/local/share/trivy/templates/html.tpl" -o report.html dakshit0/sprint-boot-app:latest '
	}
}
stage('Upload Scan report to AWS S3') {
	steps {
		sh 'cp report.html /home/dakshit/devsecops-project/'
	}	
}
stage('Docker Push') {
      steps {
   sh "docker login -u ${username} -p ${password} "
   sh 'docker push dakshit0/sprint-boot-app:v1.$BUILD_ID'
   sh 'docker push dakshit0/sprint-boot-app:latest'
   sh 'docker rmi dakshit0/sprint-boot-app:v1.$BUILD_ID dakshit0/sprint-boot-app:latest'
   }
     }
stage('Deploy to k8s') {
   steps {
        script{
                     kubernetesDeploy configs: 'spring-boot-deployment.yaml', kubeconfigId: 'kubernetes'
   	}
   }
}
post{
   always{
   	sendSlackNotifcation()
   }
}
def sendSlackNotifcation()
{
if ( currentBuild.currentResult == "SUCCESS" ) {
buildSummary = "Job_name: ${env.JOB_NAME}\n Build_id: ${env.BUILD_ID} \n Status: *SUCCESS*\n Build_url: ${BUILD_URL}\n Job_url: ${JOB_URL} \n"
slackSend( channel: "#devsecops-slack-1", token: 'devsecops-slack-1', color: 'good', message: "${buildSummary}")
}
else {
buildSummary = "Job_name: ${env.JOB_NAME}\n Build_id: ${env.BUILD_ID} \n Status: *FAILURE*\n Build_url: ${BUILD_URL}\n Job_url: ${JOB_URL}\n \n "
slackSend( channel: "#devsecops-slack-1", token: 'devsecops-slack-1', color : "danger", message: "${buildSummary}")
}
}
