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
		stage('Scan image') {
            steps {
            // Scan policy is managed in the Compute Console
                prismaCloudScanImage ca: '',
                cert: '',
                dockerAddress: "${env.DOCKER_ADDR}",
                ignoreImageBuildTime: true,
                image: "${env.IMAGE_NAME}:${env.BUILD_NUMBER}",
                key: '',
                logLevel: 'debug',
                podmanPath: '',
                project: '',
                resultsFile: 'prisma_cloud_scan_results.json'
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
