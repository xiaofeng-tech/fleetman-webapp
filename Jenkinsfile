pipeline {
   agent any

   environment {
     // You must set the following environment variables
     ORGANIZATION_NAME = "xiaofeng-tech"
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "fleetman-webapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
//             git credentialsId: 'xiaofengzs', url: "git@github.com:xiaofeng-tech/fleetman-webapp.git"
            withCredentials([sshUserPrivateKey(credentialsId: 'git-pull-with-ssh', keyFileVariable: 'SSH_KEY')]) {
              sh 'GIT_SSH_COMMAND="ssh -i $SSH_KEY" git clone git@github.com:xiaofeng-tech/fleetman-webapp.git'
            }
         }
      }
      stage('Build') {
         steps {
            sh 'echo No build required for Webapp.'
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
