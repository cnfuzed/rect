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
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
