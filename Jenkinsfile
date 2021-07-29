def commit_id
pipeline {
	agent any
	stages{
		stage('preparation'){
			steps{  checkout scm
				sh "git rev-parse --short HEAD > .git/commit-id"
				script {
				commit_id = readFile ('.git/commit-id').trim()
					}
				}	
			}
		stage('build'){
			steps{ echo '1. BUILDING MAVEN WOKRLOAD'
				sh "mvn clean install"
				echo 'Build completed' }	
	 		}
		
		stage('image'){
			steps{ echo '2. BUILDING DOCKER image'
				sh "docker build -t position-simulator:${commit_id} ."
				echo 'Build completed'}	
			}
		stage('deploy'){
			steps{ echo '3. DEPLOYMENT to kubernetes'
				sh "sed -i -r 's|richardchesterwood/position-simulator:release2|position-simulator:${commit_id}|' ./assignment_6/workloads.yaml"
				sh "kubectl get all"
				sh "kubectl apply -f ./assignment_6/workloads.yaml "
				sh "kubectl get all"
				echo "deployment completed"	
			}
	}
	}
} 

