pipeline{

    agent any

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
    }
}