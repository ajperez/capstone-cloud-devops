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
				}
			}
		}

		

		stage('Create config file for cluster') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					echo '''
						crear config file para el cluster
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

        stage ('Green deployment to AWS Loadbalancer') {
            steps {
               withAWS(region:'us-east-1', credentials:'aws-static') {
                   echo '''
				   		subo desarrollo verde al balanceador de carga
				   '''
					sh 'kubectl apply -f green-deploy.yml'

               }
            }
        }

        stage ('Remove blue deployment from AWS Loadbalancer') {
            steps {
               withAWS(region:'us-east-1', credentials:'aws-static') {
                   echo '''
				   		borro desarrollo azul del balanceador de carga
				   '''
				   sh 'kubectl delete deploy/deployment-blue'
               }
            }
        }

        stage ('Add blue deployment to AWS Loadbalancer') {
            steps {
               withAWS(region:'us-east-1', credentials:'aws-static') {
                   echo '''
				   		subo desarrollo azul al balanceador de carga
				   '''
				   sh 'kubectl apply -f blue-deploy.yml'
               }
            }
        }

        stage ('Remove green deployment from AWS Loadbalancer') {
            steps {
               withAWS(region:'us-east-1', credentials:'aws-static') {
                   echo '''
				   		borro desarrollo verde del balanceador de carga
				   '''
				   sh 'kubectl delete deploy/deployment-green'
               }
            }
        }
    }
}