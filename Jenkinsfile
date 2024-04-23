pipeline {
	
	agent any
	
	stages {
	
		stage('Build Application') {
			
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
				
				withCredentials([
				    usernamePassword(credentialsId: 'anypoint_credentials', usernameVariable: 'ANYPOINT_USERNAME', passwordVariable: 'ANYPOINT_PASSWORD'), 
				    usernamePassword(credentialsId: 'anypoint_platform_uoc', usernameVariable: 'ANYUOC_USERNAME', passwordVariable: 'ANYUOC_PASSWORD')
				]) {
					sh 'mvn clean deploy -DmuleDeploy -Denv="Sandbox" -Danypoint.username=${ANYUOC_USERNAME} -Danypoint.password=${ANYUOC_PASSWORD} -Dapp.name="Shopify"'
				
				}				
			
			}
		
		}
	
	}

}
