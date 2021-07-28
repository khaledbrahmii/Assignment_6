def commit_id
pipeline {
	agent any

	stages {

		stage('Preparation') {
			steps {
				checkout scm
				sh "git rev-parse --short HEAD> .git/commit-id"
				script {
					commit_id = readFile('.git/commit-id').trim()
				}
			}
		}

		stage('Image Build') {
			steps {
				echo 'Building ...'
				sh 'scp -r -i $(minikube ssh-key) ./* docker@$(minikube ip):~/'
				sh "minikube ssh 'docker build -t webapp:${commit_id} ./'"
				echo 'Build Complete'
			}
		}

		stage('Deploy') {
			steps {
				echo 'Deploying to Kubernetes'
 				sh "sed -i -r 's|richardchesterwood/k8s-fleetman-webapp-angular:release2|webapp:${commit_id}$|' ./assignment_6/workloads.yaml"
				sh 'kubectl get all'
				sh 'kubectl apply -f ./assignment_6/'
				sh 'kubectl get all'
				echo 'Deployment Complete'
			}	
		}
	}
}
