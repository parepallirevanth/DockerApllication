pipeline {
	agent any
	stages {
		
		stage('Clone Repository') {
			steps {
				sh ''' #! /bin/bash
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@52.229.23.58 '
                                sudo rm -rf JenkinsPipeline 
				'
				scp -r /var/lib/jenkins/workspace/JenkinsPipeline RemoteJenkin@52.229.23.58:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@52.229.23.58 '
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
				ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/id_rsa RemoteJenkin@52.229.23.58 '
				echo y | sudo apt-get install pass gnupg2
				docker login -u revanthparepalli -p Reva@1998
				docker tag jenkinspipeline_chatapp:latest revanthparepalli/jenkinspipeline_chatapp:latest
				docker push revanthparepalli/jenkinspipeline_chatapp:latest
				'
				'''
			}
		}
		

	}
	
}



