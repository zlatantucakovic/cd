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
		stage('Pull image from Docker Hub') {
			agent{ label 'master' }
			steps{
				sh 'docker pull nginx:latest' 
			}
		}

		stage('Push image') {
			agent{ label 'master' }
			steps{
				sh 'curl -X GET 128.131.58.63:5000/v2/nginx/tags/list'
				script{
					docker.withRegistry(registry) {
						dockerImage = docker.image("nginx:latest")
						dockerImage.push("${env.BUILD_NUMBER}")
						dockerImage.push("latest")
					}
				sh 'curl -X GET 128.131.58.63:5000/v2/nginx/tags/list'
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
				//sh 'cp /home/kub/newfolder/myweb.yaml $(pwd)/myweb.yaml'
				sh 'pwd'
				sh 'cd /root/newfolder'
				sh 'pwd'
				sh 'ls'
				sh 'kubectl config view'
				sh 'kubectl apply -f myweb.yaml'
				//sh 'rm myweb.yaml'
			}
		}
		
	}	
}
