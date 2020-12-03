pipeline {
    agent any
    parameters {
        choice(name: 'deployment', choices: 'green\nblue', description: 'Choose the blue/green deployment')
	}
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
							sh "docker tag spring-api:latest 813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:${params.deployment}-${BUILD_NUMBER}"
							sh "docker push 813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:${params.deployment}-${BUILD_NUMBER}"
							}
					}
				}
			
		
		}
        stage('Spring Deploy') {
            steps {
		    script {
		        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "AWS", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                           sh 'aws eks --region us-west-2 update-kubeconfig --name eks-cluster'
			   sh "sed -i \"s;813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:latest;813213957333.dkr.ecr.us-west-2.amazonaws.com/capstone:${params.deployment}-${BUILD_NUMBER};g\" capstone-deployment-${params.deployment}.yml"
		           sh "kubectl apply -f capstone-deployment-${params.deployment}.yml"
			   sh "sed -i \"s;DEPLOYMENT;${params.deployment};g\" service.yaml"
			   sh 'kubectl apply -f service.yaml'
			}
		 }
	     }
        }        
    }
}
