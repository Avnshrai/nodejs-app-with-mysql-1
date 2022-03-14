pipeline{
	agent any
	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-node-app')
	}

	stages {
		stage('Build image for frontend') {
			steps {
				sh "docker build -t giitcodes/frontend:v${env.BUILD_ID} frontend/."
			}
		}

		stage('Build image for backend') {
			steps {
				sh "docker build -t giitcodes/backend:v${env.BUILD_ID} backend/."
			}
		}
		
		stage('Build image for mysql') {
			steps {
				sh "docker build -t giitcodes/mysqldb:v${env.BUILD_ID} mysqldb/."
			}
		}

		stage('Login to Dockerhub') {
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		
		stage('Push all images') {
			steps {
				sh "docker push frontend:v${env.BUILD_ID}"
				sh "docker push backend:v${env.BUILD_ID}"
				sh "docker push mysqldb:v${env.BUILD_ID}"
			}
		}
	}
	
	post {
		always {
			sh "docker logout"
		}
	}
}
