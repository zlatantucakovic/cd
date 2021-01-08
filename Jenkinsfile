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
		stage('Build') {
			agent {
				docker {
					image 'maven:3-alpine'
					args '-u root'
				}
			}
			steps {
				sh 'mvn -B -DskipTests clean package'
				sh 'mvn test'
				sh './jenkins/scripts/deliver.sh'
				stash includes: 'target/my-app-1.0-SNAPSHOT.jar', name: 'ARTEFACT'
			}
		}

		stage('Build image') {
			agent{ label 'master' }
			steps {
				dir("ARTEFACT"){	
					unstash 'ARTEFACT'
				}
				sh "ls -la ${pwd()}"
				sh "ls -la ${pwd()}/ARTEFACT"
				sh 'docker build -t jovan:1.0 .'
				//script{
				//	dockerImage = docker.build "jovan:1.0"				
				//}
			}	
	  	}
		stage('Push image') {
			agent{ label 'master' }
			steps{
				//sh "docker tag jovan:1.0 registry.localhost:5000/jovan:${env.BUILD_NUMBER}"
				//sh "docker push registry.localhost:5000/jovan:${env.BUILD_NUMBER}"
				//sh "docker tag jovan:1.0 registry.localhost:5000/jovan:latest"
				//sh "docker push registry.localhost:5000/jovan:latest"
				//sh 'docker login'
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
				//sh "docker pull registry.localhost:5000/jovan:latest"
				//sh 'docker login'
				script{
					docker.withRegistry(registry) {
						dockerImage2 = docker.image("jovan:latest")
						dockerImage2.pull()
					}
				} 
			}
		}
/*
		stage('K3d') {
			agent{ label 'master' }
			steps{
				sh 'docker volume create local_registry'
				sh 'docker container rm -f registry.localhost'
				sh 'docker container run -d --name registry.localhost -v local_registry:/var/lib/registry --restart always -p 7000:5000 registry:2'
				sh 'docker pull nginx:latest'
				sh 'docker tag nginx:latest registry.localhost:7000/nginx:latest'
				sh 'docker push registry.localhost:7000/nginx:latest'
				sh 'docker network connect k3d-k3s-default registry.localhost'
				sh 'k3d delete --name klaster'
				sh 'k3d create --api-port 6555 --publish 8085:80 --workers 2 --registry-name registry.localhost --registry-port 7000 --name klaster'				
				withKubeConfig([credentialsId: "kubeconfig", serverUrl: "https://localhost:6550"]){
					sh 'export KUBECONFIG="$(k3d get-kubeconfig --name="klaster")"'
					echo '$KUBECONFIG'
				}
//				sh 'export KUBECONFIG=\"$(k3d get-kubeconfig --name=\"klaster\")\"'
				sh 'kubectl delete deployment nginx'
				sh 'kubectl create deployment nginx --image=nginx'
				sh 'kubectl delete service nginx'
				sh 'kubectl create service clusterip nginx --tcp=80:80'
				sh 'ls'
				sh 'kubectl apply -f ingresscont.yml'
				sh 'curl localhost:8085/'
			}
		}

		
		stage('Kubernetes deploy') {
		//	agent {
		//		kubernetes {
		//			yaml """
		//			<A long yaml file>
		//			"""
		//		}
		//	}
			agent { label 'master' }
			steps{
				sh "minikube start"
//				sh "minikube node add --worker=true"
			}
		}
		
	*/	
		stage('Run image'){
			agent{ label 'master' }
			steps{
				//sh 'pwd'
				//sh 'export PATH="$HOME/go/bin/:$PATH"'
				//sh 'kubectl version'
				//sh 'kubectl get nodes'
				//sh 'kubectl config view'
				//sh 'minikube start'
				//sh 'minikube unpause'
				//sh 'minikube pause'
				sh 'kubectl config view'
		//		sh 'docker login'
		//		script{
		//			docker.withRegistry(registry) {
		//				dockerImage = docker.build "jovan:2.0"
		//				dockerImage.push()
		//			}
		//		} //mozda htttpS
				//withKubernetes(serverUrl: "https://192.168.49.2:9000", credentialsId: "kubbb2") {
				//	sh 'kubectl run jovan --image=http://128.131.58.63:5000/jovan:latest --port=8080'
				//}
			}
		}
	}	
}
