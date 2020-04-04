pipeline {
    
    agent any

    environment
    {
        PATH="/opt/apache-maven-3.6.3/bin:$PATH"
	registry = "nevincleetus/java-web-app-cicd"
        registryCredential = 'dockerhub'
        dockerImage = ''    
    }
	
    stages {
    	stage('Build') {
          agent {
            docker {
               image 'maven:3-alpine'
               args '-v /root/.m2:/root/.m2'
            }
          }

            steps {
                sh 'mvn -B -DskipTests clean package'
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
                sh 'mvn test'
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
