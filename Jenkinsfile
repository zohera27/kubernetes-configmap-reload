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

                    mvntest()
                }           
                
            }
        }




    }
}