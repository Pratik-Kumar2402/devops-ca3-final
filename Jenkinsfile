pipeline {
   agent any
  
   environment {
       DOCKER_HUB_REPO = "pratik2402/devops-ca3"
       CONTAINER_NAME = "devops-ca3"
       DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
   }
  
   stages {
       /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */
      
       stage('Build') {
           steps {
               echo 'Building..'
               sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
           }
       }
       stage('Test') {
           steps {
               echo 'Testing..'
               sh 'docker stop $CONTAINER_NAME || true'
               sh 'docker rm $CONTAINER_NAME || true'
               sh 'docker run --name $CONTAINER_NAME $DOCKER_HUB_REPO /bin/bash -c "pytest test.py"'
           }
       }
       stage('Push') {
           steps {
               echo 'Pushing image..'
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
               sh 'docker push $DOCKER_HUB_REPO:latest'
           }
       }
       stage('Deploy') {
           steps {
               echo 'Deploying....'
		   script{
			   sh('ls')
			// kubernetesDeploy (configs: 'deployment.yaml', kubeconfigId: 'kubernetes-config')
			// kubernetesDeploy (configs: 'service.yaml', kubeconfigId: 'kubernetes-config')
			sh 'minikube kubectl -- apply -f deployment.yaml'
			   
		   }
	       
           }
       }
   }
}
