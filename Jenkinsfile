pipeline {
	environment {
		registry = "http://128.131.58.63:5000" 
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
						dockerImage = docker.build ("jovan")
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
						dockerImage2 = docker.image("jovan:latest")
						dockerImage2.pull()
					}
				} 
			}
		}
	
		stage('Deploy image'){
			agent{ label 'master' }
			steps{
//				sh 'cd newfolder'
				sh 'kubectl config view'
				sh 'kubectl apply -f myweb.yaml'
			}
		}
	}	
}
