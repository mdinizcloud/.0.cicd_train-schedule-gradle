pipeline {
    agent any

    environment { 
        CC = 'clang'
        SSH_CREDS = credentials('root-k8s-control-plane')
        JENKINS_CREDS = credentials('jenkins-admin')
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
     
        stage('Curl Passwd') {
           environment { 
                DEBUG_FLAGS = '-g'
            }
            steps {
                // echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                // echo "USERNAME DOUBLE ${SSH_CREDS_USR} on ${SSH_CREDS_PSW}"
                // echo "Single Quote Begin...."
                // echo 'USERNAME SINGLE $SSH_CREDS_USR on $SSH_CREDS_PSW'
                // sh 'printenv'
            sh('curl -u $JENKINS_CREDS_USR:$JENKINS_CREDS_PSW http://jenkins.cybertron.corp')
            }
        }
    
    }
}