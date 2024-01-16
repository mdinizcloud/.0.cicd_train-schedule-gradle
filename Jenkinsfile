pipeline {
    agent any

    environment { 
        CC = 'clang'
        SSH_CREDS = credentials('root-k8s-control-plane')

    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
          when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
           }
            steps {
                echo 'Deploying....'
            }
        }
     
        stage('Example') {
           environment { 
                DEBUG_FLAGS = '-g'
            }
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"

                echo "USERNAME DOUBLE ${SSH_CREDS_USR} on ${SSH_CREDS_PSW}"
                echo "Single Quote Begin...."
                echo 'USERNAME SINGLE $SSH_CREDS_USR on $SSH_CREDS_PSW'
                // sh 'printenv'
            }
        }
    
    }
}