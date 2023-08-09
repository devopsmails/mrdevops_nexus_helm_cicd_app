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
                withCredentials([string(credentialsId: 'nexus_cred_pwd', variable: 'nexus_pwd')])
                script{
                    docker build -t 34.238.232.93:8083/springapp:${VERSION} .

                    docker login -u admin -p  $nexus_pwd 34.238.232.93:8083
                    docker push 34.238.232.93:8083/springapp:${VERSION}
                    docker rmi 34.238.232.93:8083/springapp:${VERSION}
                }
            }
        }
    }
}