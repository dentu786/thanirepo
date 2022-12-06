pipeline {
	agent any
	environment {
		PROJECT_ID = 'focusedservices-nam'
		CLUSTER_NAME = 'jenkins-cd'
		LOCATION = 'us-east1-d'
		CREDENTIALS_ID = 'FocusedServices-NAM'
	}
	stages {
		stage("Checkout code") {
			steps {
				checkout scm
			}
		}
		stage("Build image") {
			steps {
				script {
					myapp = docker.build("jdentu/raheelrepo:${env.BUILD_ID}")
				}
			}
		}
		stage("Push image") {
			steps {
				script {
					docker.withRegistry('https://registry.hub.docker.com', 'dockerID') {
						myapp.push("latest")
						myapp.push("${env.BUILD_ID}")
					}
				}
			}
		}

	}

}
