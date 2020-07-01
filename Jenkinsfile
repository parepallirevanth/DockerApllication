pipeline {
	agent any
	stages {
		stage('Sonarqube') {
                  	environment {
                       		scannerHome = tool 'sonarScanner'
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
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@137.116.45.66 '
                                sudo rm -rf JenkinsPipeline 
				'
				scp -r /var/lib/jenkins/workspace/JenkinsPipeline root@137.116.45.66:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@137.116.45.66 '
				cd JenkinsPipeline
				docker stop $(docker ps -a -q)
				docker rm $(docker ps -a -q)
				docker rmi jenkinspipeline_nginx:latest jenkinspipeline_chatapp:latest revanthparepalli/jenkinspipeline_chatapp:latest 
				docker-compose up -d
				'
				'''
			}
		}
		stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@137.116.45.66 '
				sudo apt-get install pass gnupg2
				docker login -u revanthparepalli -p Reva@1998
				docker tag jenkinspipeline_chatapp:latest revanthparepalli/jenkinspipeline_chatapp:latest
				docker push revanthparepalli/jenkinspipeline_chatapp:latest
				'
				'''
			}
		}
		

	}
	post {
		always {
			echo 'Stage is success'
		}
	}
}



