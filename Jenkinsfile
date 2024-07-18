pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git 'https://github.com/Ins1ght32/D3.git'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML --nvdApiKey 7ad48849-c21a-49f4-9ddb-85151d39d039 --noupdate --enableExperimental', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
			}
		}
		
		stage('Code Quality Check via SonarQube') {
			steps {
				script {
					def scannerHome = tool 'SonarQube';
						withSonarQubeEnv('SonarQube') {
						sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=D3 -Dsonar.sources=. -Dsonar.host.url=http://192.168.1.188:9000 -Dsonar.token=192.168.1.188"
						}
					}
				}
		}
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}
