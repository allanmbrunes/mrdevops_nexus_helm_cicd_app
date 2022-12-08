pipeline{
    agent any
    //     environment {
    //     api_jenkins_sq = 'sonarqube_secret'
        
    // }
        stages{
            stage('sonar quality check'){
                steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'sonarqube_secret' ) {
                            sh 'mvn clean package sonar:sonar'                         
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