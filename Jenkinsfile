pipeline{
    agent any
        environment {
        api_jenkins_sq = 'sonarqube_secret'
        
    }
        stages{
            stage('sonar quality check'){
                    steps{
                        script{
                            withSonarQubeEnv(credentialsId: "${api_jenkins_sq}") {
                                sh 'mvn clean package sonar:sonar'                         
                            }
                        }
                    }
        }
    }
}