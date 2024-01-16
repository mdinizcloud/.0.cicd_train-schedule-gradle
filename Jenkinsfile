pipeline {
    agent any

    environment { 
        CC = 'clang'
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
                // echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'printenv'
            }
        }
    
    }
}