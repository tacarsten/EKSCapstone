pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Lint Dockerfile') {
            steps {
                sh 'docker run --rm -i hadolint/hadolint < Dockerfile'     
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
                sh 'ls -a'      
            }
        }      
		stage ("Push") { 
			steps {
					script {
						withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "AWS", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
							def awsLogin  = sh(script: "aws ecr get-login --region us-west-2 --no-include-email", returnStdout: true)
                                                        sh "${awsLogin}"
							sh 'docker build -t spring-api .'
							sh 'docker tag spring-api:latest 813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:latest'
							sh 'docker push 813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:latest'
							}
					}
				}
			
		
		}
        stage('Spring Deploy') {
            steps {
		    script {
		        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "AWS", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                           sh 'aws eks --region us-west-2 update-kubeconfig --name eks-cluster'
		           sh 'kubectl apply -f capstone-deployment.yml'
			}
		 }
	     }
        }        
    }
}
