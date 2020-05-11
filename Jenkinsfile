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
					sudo sh '''
						docker build -t ajpm1977/capstone-devops .
					'''
				}
			}
		}

		stage('Push Image To Dockerhub') {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sudo sh '''
						docker push ajpm1977/capstone-devops
					'''
				}
			}
		}

		stage('Create AWS kubernetes cluster') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					sh '''
						echo 'Crear kubernetes cluster'
					'''
				}
			}
		}

		

		stage('Create config file for cluster') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					sh '''
						echo 'crear config file para el cluster'
					'''
				}
			}
		}

		stage('Set current kubectl context') {
			steps {
				withAWS(region:'us-east-1', credentials:'aws-static') {
					sh '''
						echo 'asigno el contexto AWS a kubectl'
					'''
				}
			}
		}

        stage ('Upload latest green deployment to AWS Loadbalancer') {
            steps {
               script {
                   sh '''
				   echo 'subo desarrollo verde al balanceador de carga'
				   '''
               }
            }
        }

        stage ('Remove old blue deployment from AWS Loadbalancer') {
            steps {
               script {
                   sh '''
				   echo 'borro desarrollo azul del balanceador de carga'
				   '''
               }
            }
        }

        stage ('Add latest blue deployment to AWS Loadbalancer') {
            steps {
               script {
                   sh '''
				   echo 'subo desarrollo azul al balanceador de carga'
				   '''
               }
            }
        }

        stage ('Remove old green deployment from AWS Loadbalancer') {
            steps {
               script {
                   sh '''
				   echo 'borro desarrollo verde del balanceador de carga'
				   '''
               }
            }
        }
    }
}