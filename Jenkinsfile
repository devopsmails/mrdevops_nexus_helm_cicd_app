pipeline{

    agent any
    environment{

        VERSION="${env.BUILD_ID}"

    }
    stages {

        stage ('Sonar Quality Check'){

            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh " mvn clean package sonar:sonar"
                    }

                }
            }
        }

        stage ('Sonar Quality Gate status'){

            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage ('Docker image build & docker push Nexus repo'){

            steps{
                
                script{
                    withCredentials([string(credentialsId: 'nexus_cred_pwd', variable: 'nexus_pwd')]) {
                        sh '''
                            docker build -t 34.238.232.93:8083/springapp:${VERSION} . 

                            docker login -u admin -p  $nexus_pwd 34.238.232.93:8083
                            docker push 34.238.232.93:8083/springapp:${VERSION}
                            docker rmi 34.238.232.93:8083/springapp:${VERSION}
                        '''
                    }
                }
            }
        }
    }
    post {
		always {
			mail bcc: '',
            body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", 
            cc: '', 
            charset: 'UTF-8', 
            from: '', mimeType: 'text/html', 
            replyTo: '', 
            subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", 
            to: "devopsmails1@gmail.com";  
		}
	}
}