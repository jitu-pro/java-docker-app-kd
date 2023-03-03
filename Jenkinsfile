pipeline {
	agent any
	stages {
		stage("SCM") {
			steps {
				git 'https://github.com/jitu9772/java-docker-app.git'
				}
			}

		stage("build") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
				}
			}
		stage("Image") {
			steps {
				sh 'sudo docker build -t java-repo:$BUILD_TAG .'
				sh 'sudo docker tag java-repo:$BUILD_TAG jitu/pipeline-java:$BUILD_TAG'
				}
			}
				
	
		stage("Docker Hub") {
			steps {
				withCredentials([string(credentialsId: 'docker_hub_passwd1', variable: 'docker_hub_passwd_var')]) {
   				 // some block
					
			
				sh 'sudo docker login -u jeetusharma -p ${docker_hub_passwd_var}'
				sh 'sudo docker push jitu/pipeline-java:$BUILD_TAG'
				}
			}	

		}
		stage("QAT Testing") {
			steps {
				sh 'sudo docker run -dit -p 8001:8080  jitu/pipeline-java:$BUILD_TAG'
				}
			}
		stage("testing website") {
			steps {
				retry(5) {
				sh 'curl --silent http://3.131.100.189:8001/java-web-app/'
					}
				}
			}

		stage("Approval status") {
			steps {
				script {
					 Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput
					}
				}	
		}
		
	}
}

