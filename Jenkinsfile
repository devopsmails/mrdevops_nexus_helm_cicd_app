pipeline{

    agent any

    stages {

        stage ('Sonar Quality Check'){

            agent {

                docker {
                    image 'maven'
                }
            }

            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh "mkdir -p /.m2/repository && mvn clean package sonar:sonar"
                    }

                }
            }
        }
    }
}