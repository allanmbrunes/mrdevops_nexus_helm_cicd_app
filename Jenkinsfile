pipeline{
    agent any
        environment {
        api_jenkins_sq = 'sonarqube_secret'
        
    }
        stages{
            stage('sonar quality check'){
/*                 agent{
                    docker {

                        image 'maven'
                    }
                } */
                    steps{
                        script{
                            withSonarQubeEnv(credentialsId: "${api_jenkins_sq}") {
                                sh 'mvn clean package sonar:sonar'                         
                            }
                        }
                    }
            }
            stage('Quality Gate Status'){
                steps{
                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube_secret'                        
                    }
                }      
            }
        }
    }
}