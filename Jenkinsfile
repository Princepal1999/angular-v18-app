pipeline {
    agent any
    tools {
        nodejs 'NodeJS'  // Ensure this matches your NodeJS installation name
    }
    
    
    environment {
        
            PATH = "${env.PATH}:/opt/sonar-scanner-6.1.0.4477/bin"
            
			def tag = ''
	}
    
    stages {
        
        stage ('SCM_Checkout'){
		steps {
		   git branch: 'main', url: 'https://github.com/Princepal1999/angular-v18-app.git'
		  script {
						tag = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
					}
		}
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'  // Adjust this to your Angular build command
            }
        }
        
        stage('Sonar Analysis') {
    steps {
        withSonarQubeEnv('SonarQubeServer') {
            sh """
                sonar-scanner \
                -Dsonar.projectKey=angular-app \
                -Dsonar.sources=src \
                -Dsonar.host.url=http://172.16.13.125:9000 \
                -Dsonar.login=sqa_473b9d761d20dc674196f5d2ca6a09c381f2429d
            """
        }
    }
}

		
		
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    //post {
    //    //always {
    //        cleanWs()
    //    }
    //}
}
