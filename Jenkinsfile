@Library('Jenkins_Shared_Library') _

pipeline{
    
    agent any

    parameters{
        
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose Create/Destroy')
        string(name: 'ImageName', description: "Name of the Docker Build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "Tag of the Docker Build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "Dokcer HUB User Account", defaultValue: 'zohera27')
        
    }

    stages{

        

        stage('Git Checkout') {

         when { expression { params.action == 'create' } }

            steps{

                script{

                    gitCheckout(
                        branch: "main",
                        url: "https://github.com/zohera27/kubernetes-configmap-reload.git"
                    )
                }
            }
        }

        stage('Unit Test maven') {

         when { expression { params.action == 'create' } }

            steps{

                script{

                    mvnTest()
                }           
                
            }
        }

        stage('Integration Test maven') {

         when { expression { params.action == 'create' } }

            steps{

                script{

                    mvnIntegrationtest()
                }           
                
            }
        }

        stage('Static Code Analysis : SonarQube') {

         when { expression { params.action == 'create' } }

            steps{

                script{

                    def SonarQubeCredentialsId = 'sonar-api'
                    StaticCodeAnalysis(SonarQubeCredentialsId)
                }           
                
            }
        }

        stage('Quality Gate Status : SonarQube') {

         when { expression { params.action == 'create' } }

            steps{

                script{
                    
                    def SonarQubeCredentialsId = 'sonar-api'
                    QualityGateStatus(SonarQubeCredentialsId)
                }           
                
            }
        }

        stage('Maven Build : Project') {

         when { expression { params.action == 'create' } }

            steps{

                script{

                    sh 'echo "JAVA_HOME=${JAVA_HOME}"'
                    
                    mvnbuild()
                }           
                
            }
        }                

    }
}