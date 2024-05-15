pipeline {
  agent any
  tools {
    jdk 'Java17'
    maven 'Maven3'
  }
   environment {
        // Định nghĩa tên image và tag, cũng như credentialsId cho Docker Hub
        IMAGE_NAME = 'ntmanh90ksth/test-java'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS_ID = 'dockerhub-ntmanh90'
    }

  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()
      }
    }

    stage("Checkout from SCM") {
      steps {
         git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ashfaque-9x/register-app'
      }
    }

    stage("Build Application") {
      steps {
        sh "mvn clean package"
      }
    }

    stage("Test Application") {
      steps {
        sh "mvn test"
      }
    }
      stage("SonarQube Analysis") {
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
	stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image từ Dockerfile trong thư mục hiện tại
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
  }
}
