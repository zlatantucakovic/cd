pipeline {
	environment {
		//registry = "http://128.130.39.40:5000" 
		registry = "http://128.131.58.63:5000" 
		//registry= "zlatantucakovic/jovan"
		registryCredential = 'dockerhub'
		dockerImage = ''
		dockerImage2 = ''

	}
	 
	agent none
	
	stages {
		stage('Push image') {
			agent{ label 'master' }
			steps{
				 
				script{
					docker.withRegistry(registry) {
						dockerImage = docker.image("nginx:latest")
						dockerImage.push("${env.BUILD_NUMBER}")
						dockerImage.push("latest")
					}
				}
			}
		}

		stage('Pull image') {
			agent{ label 'master' }
			steps{
				script{
					docker.withRegistry(registry) {
						dockerImage2 = docker.image("nginx:latest")
						dockerImage2.pull()
					}
				} 
			}
		}
	
		stage('Deploy image'){
			agent{ label 'master' }
			steps{
				//sh 'cd newfolder'
				sh 'kubectl config view'
				sh 'kubectl apply -f myweb.yaml'
			}
		}
		
	}	
}
