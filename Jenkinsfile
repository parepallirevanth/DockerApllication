pipeline {
	agent any
	stages {
		stage('Clone Repository') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@40.70.83.87 

				scp -r /var/lib/jenkins/workspace/JenkinsPipeline root@40.70.83.87:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@40.70.83.87 '
				cd JenkinsPipeline
				docker-compose up -d
				'
				'''
			}
		}
		stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@40.70.83.87 '
				'
				'''
			}
		}
		stage('Deploy') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa ubuntu@13.233.255.236 '
				'
				echo Deploy Successfull
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



