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
    }
}