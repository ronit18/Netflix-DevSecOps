pipeline{
	agent any
	tools{
		jdk 'jdk7'
		nodejs 'node16'
	}
	environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
	stages{
		stage('Clean workspace'){
			steps{
				cleanWS()
			}
		}
		stage('Checkout from GIT'){
			steps{
				git branch: 'main', url:'https://github.com/ronit18/Netflix-DevSecOps/'
			}
		}
		stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
            }
		}
 		stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
}