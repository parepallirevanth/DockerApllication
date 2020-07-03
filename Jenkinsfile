pipeline {
	agent any
	stages {
		stage('Sonarqube') {
                  	environment {
                       		scannerHome = tool 'sonar-scanner'
                       	}
                	steps {
                   		withSonarQubeEnv('sonar') {
                       		sh "${scannerHome}/bin/sonar-scanner"
                   		}
                   		timeout(time: 5, unit: 'MINUTES') {
                       		waitForQualityGate abortPipeline: true
				}
                	}
                }
		stage('Clone Repository') {
			steps {
				sh ''' #! /bin/bash
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@20.36.20.82 '
                                sudo rm -rf Pipeline 
				'
				scp -r /var/lib/jenkins/workspace/Pipeline RemoteJenkin@20.36.20.82:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@20.36.20.82 '
				cd Pipeline
				docker stop $(docker ps -a -q)
				docker rm $(docker ps -a -q)
				docker rmi pipeline_nginx:latest pipeline_chatapp:latest revanthparepalli/pipeline_chatapp:latest 
				docker-compose up -d
				'
				'''
			}
		}
		stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@20.36.20.82 '
				echo y | sudo apt-get install pass gnupg2
				docker login -u revanthparepalli -p Reva@1998
				docker tag pipeline_chatapp:latest revanthparepalli/pipeline_chatapp:latest
				docker push revanthparepalli/pipeline_chatapp:latest
				'
				'''
			}
		}
		

	}
	
}



