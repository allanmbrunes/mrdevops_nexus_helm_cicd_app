pipeline{
    agent any
        environment {
        VERSION = "${env.BUILD_ID}"
        
    }
        stages{
            stage('sonar quality check'){
                steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'sonarqube_secret' ) {
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
            stage('Docker build & docker push to Nexus repo'){
                steps{
                    script{
                        withCredentials([string(credentialsId: 'Nexus_pass', variable: 'nexus')])
                        sh '''
                         docker build -t localhost:8081/springapp:${VERSION} .
                         
                         docker login -u admin -p $Nexus_pass localhost:8081

                         docker push localhost:8081/springapp:${VERSION}

                         docker rmi localhost:8081/springapp:${VERSION}
                        '''
                    }
                } 


            }
        }
    }
