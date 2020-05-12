pipeline {
	agent any
	stages {

		stage('Lint HTML') {
			steps {
				sh 'tidy -q -e ./index.html'
			}
		}
		
		stage('Build Docker Image') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					echo '''
						build docker image
					'''
				}
			}
		}

		stage('Push Image To Dockerhub') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					echo '''
						push image to dockerhub
					'''
				}
			}
		}

		stage('Create AWS kubernetes cluster') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					echo '''
						Crear kubernetes cluster
					'''
					sh '''

						eksctl create cluster \
						--name cluster-ajpm \
						--version 1.13 \
						--nodegroup-name standard-workers \
						--node-type t2.small \
						--nodes 2 \
						--nodes-min 1 \
						--nodes-max 3 \
						--node-ami auto \
						--region us-east-1 \
						--zones us-east-1a \
						--zones us-east-1b \
						--zones us-east-1c \
					'''
				}
			}
		}

		

		stage('Create config file for cluster') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					echo '''
						crear config file para el cluster
					'''
					sh '''
						aws eks --region us-east-1 update-kubeconfig --name cluster-ajpm
					'''					
				}
			}
		}

		stage('Set current kubectl context') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					echo '''
						asigno el contexto AWS a kubectl
					'''
				}
			}
		}

        stage ('Upload latest green deployment to AWS Loadbalancer') {
            steps {
               script {
                   echo '''
				   		subo desarrollo verde al balanceador de carga
				   '''
               }
            }
        }

        stage ('Remove old blue deployment from AWS Loadbalancer') {
            steps {
               script {
                   echo '''
				   		borro desarrollo azul del balanceador de carga
				   '''
               }
            }
        }

        stage ('Add latest blue deployment to AWS Loadbalancer') {
            steps {
               script {
                   echo '''
				   		subo desarrollo azul al balanceador de carga
				   '''
               }
            }
        }

        stage ('Remove old green deployment from AWS Loadbalancer') {
            steps {
               script {
                   echo '''
				   		borro desarrollo verde del balanceador de carga
				   '''
               }
            }
        }
    }
}