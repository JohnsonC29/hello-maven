pipeline {
    
    agent any

    environment
    {        
	registry = "nevincleetus/helloworld-repo"
        registryCredential = 'dockerhub'
        dockerImage = ''  
    }		
    tools { 
        maven 'M2_HOME'         
    }		
    stages {	    
	stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }	
	stage ('Build') {
            steps {
                sh 'mvn clean package'
            }         
        }    
	
       stage('Test') {
            agent {
               docker {
                   image 'maven:3-alpine'
                   args '-v /root/.m2:/root/.m2'
               }
            }
            steps {
                sh 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
       }
	    
      stage('Building image') {
          steps{
              script {
                  dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
          }
      }	 
    }	
}
