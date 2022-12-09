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
                        withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                            sh '''
                                docker build -t localhost:8083/springapp:${VERSION} .
                                
                                docker login -u admin -p $nexus_creds localhost:8083

                                docker push localhost:8083/springapp:${VERSION}

                                docker rmi localhost:8083/springapp:${VERSION}
                            '''
                        }                           
                    }
                } 
            }
            stage('Identifying misconfig using datreee in helm charts'){

                steps{
                    script{
                        dir('kubernetes/myapp/') {
                            withEnv(['DATREE_TOKEN=29470161-741a-459b-a390-426790048d7e']) {
                                sh 'helm datree test .'
                            }
                            
                        }        

                    }
                    
                } 
            }
        }
    }
