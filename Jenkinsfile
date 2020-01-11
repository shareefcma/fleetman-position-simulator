pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     ECR_URI = "842970055596.dkr.ecr.us-east-1.amazonaws.com"
     
     SERVICE_NAME = "fleetman-position-simulator" 
     REPOSITORY_TAG="${ECR_URI}/leg888:${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      
      stage('Build') {
         tools {
               maven 'maven'
         }
         steps {
            
            sh '''mvn clean package'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'sudo docker image build -t ${REPOSITORY_TAG} .'
           sh 'sudo docker push ${REPOSITORY_TAG}'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
