pipeline {
  agent any 
  environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-login')
	}
  tools {
		maven 'maven3.8.2'
	}
    stages {
	    stage ('git clone') {
	    
		    steps {
		     git 'https://github.com/sasender/spring-boot-mongo-docker.git'
		  
		    }
		}
		
		stage ('mvn build') {
		    
			steps {
			  sh 'mvn --version'
			  sh 'mvn clean package'
			}
		}
		
		stage ('docker build') {
            
            steps {
                sh 'docker build -t sasender/spring-boot-mongo .'
                
            }
		}
		
		
		stage ('docker login') {
		    
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
				
		}
		
		stage ('docker push') {
		    steps {
			    sh 'docker push sasender/spring-boot-mongo'
			}	
		
		}
		
		stage ('Deploy To Kuberates Cluster') {
// 		    steps {
// 			     kubernetesDeploy (
// 				configs: 'springBootMongo.yml', 
//                 kubeconfigId: '8367a0b4-a1f9-4cd9-863e-8b8e553b3142',
//                 enableConfigSubstitution: true
//                 )
// 			}
	        steps {
	            kubeconfigId: '8367a0b4-a1f9-4cd9-863e-8b8e553b3142'
	            sh 'kubectl apply -f springBootMongo.yml'
	        }
		}
	}
}
