pipeline{

	agent any{

	environment {
		DOCKERHUB_CREDENTIALS=credentials('docker')
	}

// 	stages {
	    
// 	    stage('gitclone') {

// 			steps {
// 				git "https://github.com/Avnshrai/nodejs-app-with-mysql-1.git"
// 			}
// 		}

		stage('Build for frontend image') {

			steps {
				sh "docker build -t avnshrai/frontend:latest FrontEnd/."
				// sh "docker build -t rajputvikram/frontend:v${env.BUILD_ID} FrontEnd/."
                // sh "docker tag rajputvikram/frontend:v${env.BUILD_ID} nadeem90/frontend:latest"
			}
		}

        stage('Build for backend image') {

			steps {
				sh "docker build -t avnshrai/backend:latest backend/."
				// sh "docker build -t rajputvikram/backend:v${env.BUILD_ID} backend/."
                // sh "docker tag rajputvikram/backend:v${env.BUILD_ID} rajputvikram/backend:latest"
			}
		}

        stage('Build for mysql image') {

			steps {
				sh "docker build -t avnshrai/mysql:latest db/."
				// sh "docker build -t rajputvikram/mysql:v${env.BUILD_ID} db/."
                // sh "docker tag rajputvikram/mysql:v${env.BUILD_ID} rajputvikram/mysql:latest"
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push avnshrai/frontend:latest'
                sh 'docker push avnshrai/backend:latest'
                sh 'docker push avnshrai/mysql:latest'
			}
		}

		stage('Remove old images') {

			steps {
				sh 'docker rmi -f avnshrai/frontend:latest'
                sh 'docker rmi -f avnshrai/backend:latest'
                sh 'docker rmi -f avnshrai/mysql:latest'
			}
		}
	}
}		
