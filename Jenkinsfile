pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubaccount')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t manik212/rect:v1 .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push manik212/rect:v1'
			}
		}
	


		stage('Deploy to K8s')
		{
			steps{
				sshagent(['k8s-jenkins'])
				{
					sh 'scp -r -o StrictHostKeyChecking=no node-deployment.yaml ubuntu@3.141.16.27:/home/ubuntu/'
					
					script{
						try{
							sh 'ssh ubuntu@3.141.16.27 kubectl apply -f ./node-deployment.yaml --kubeconfig=/home/ubuntu/.kube/config'

							}catch(error)
							{

							}
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
