pipeline {
	
	agent any
	
	stages {
	
		stage('Build Application') {
			environment {
			
				ANYPOINT_CREDENTIALS = credentials('anypoint_credentials')
			
			}
			
			steps {
				echo 'Building Application'
				withCredentials([usernamePassword(credentialsId: 'anypoint_credentials', usernameVariable: 'ANYPOINT_USERNAME', passwordVariable: 'ANYPOINT_PASSWORD')]) {
				
					sh 'mvn -B package --file pom.xml --settings ./settings.xml -Denv.USERNAME=${ANYPOINT_USERNAME} -Denv.PASSWORD=${ANYPOINT_PASSWORD}'
				
				}
				
			}
		
		}
		
		stage('Test') {
		
			steps {
			
				echo 'Application in Testing Phase…'
				
				sh 'mvn test'
			
			}
		
		}
		
		stage('Deploy CloudHub') {
		
			environment {
			
				ANYPOINT_CREDENTIALS = credentials('anypoint_credentials')
			
			}
			
			steps {
			
				echo 'Deploying mule project due to the latest code commit…'
				
				echo 'Deploying to the configured environment….'
				
				sh 'mvn package deploy -DmuleDeploy -Dusername=${ANYPOINT_CREDENTIALS_USR} -Dpassword=${ANYPOINT_CREDENTIALS_PSW} -DworkerType=Micro -Dworkers=1 -Dregion=us-west-2'
			
			}
		
		}
	
	}

}
