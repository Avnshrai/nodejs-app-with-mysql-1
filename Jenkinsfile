pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('docker_hub_node_app')
	}

	stages {
	    
	    stage('gitclone') {

			steps {
				git "https://github.com/rajputvikram/nodejs-app-with-mysql.git"
			}
		}

		stage('Build for frontend image') {

			steps {
				sh "docker build -t rajputvikram/frontend:latest FrontEnd/."
				// sh "docker build -t rajputvikram/frontend:v${env.BUILD_ID} FrontEnd/."
                // sh "docker tag rajputvikram/frontend:v${env.BUILD_ID} nadeem90/frontend:latest"
			}
		}

        stage('Build for backend image') {

			steps {
				sh "docker build -t rajputvikram/backend:latest backend/."
				// sh "docker build -t rajputvikram/backend:v${env.BUILD_ID} backend/."
                // sh "docker tag rajputvikram/backend:v${env.BUILD_ID} rajputvikram/backend:latest"
			}
		}

        stage('Build for mysql image') {

			steps {
				sh "docker build -t rajputvikram/mysql:latest mysqldb/."
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
				sh 'docker push rajputvikram/frontend:latest'
                sh 'docker push rajputvikram/backend:latest'
                sh 'docker push rajputvikram/mysql:latest'
			}
		}

		stage('Remove old images') {

			steps {
				sh 'docker rmi -f rajputvikram/frontend:latest'
                sh 'docker rmi -f rajputvikram/backend:latest'
                sh 'docker rmi -f rajputvikram/mysql:latest'
			}
		}


		stage('Deploy on k8s kops') {

			steps {

				script{
					withCredentials([kubeconfigFile(credentialsId: 'kubernetes-config', variable: 'KUBECONFIG')]) {
						script{
							try{
							sh 'kubectl create -f kubernetes/namespace.yml'	
							}catch(error){
							sh 'kubectl apply -f kubernetes/namespace.yml'
							}
						}
		
						sh 'kubectl apply -f kubernetes/mysql.yml'
						sh 'kubectl apply -f kubernetes/backend.yml'
						sh 'kubectl apply -f kubernetes/frontend.yml'
					}
				} 
			 }
			}
		


	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
