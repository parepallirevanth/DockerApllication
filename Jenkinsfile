pipeline {
    agent any
    stages{  
        stage('Build-image') {
            steps {
                sh ''' #!/bin/bash
                  cd /var/lib/jenkins/workspace/chatapp/
                  #docker-compose build .
                  sudo docker-compose up -d 
                # pushing to the Docker-hub
                #  docker login -u revanthparepalli -p Reva@1998    
                #  docker tag chatapp:$BUILD_NUMBER revanthparepalli/chatapp:$BUILD_NUMBER
                #  docker push revanthparepalli/chatapp:$BUILD_NUMBER
                # docker tag chatapp:$BUILD_NUMBER revanthparepalli/chatapp:JV1
                # docker push revanthparepalli/chatapp:JV1
                  
                  '''  
            }
        }
    }
}  
 
   
