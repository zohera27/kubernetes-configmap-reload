@Library('Jenkins_Shared_Library') _

pipeline{

    agent any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the option Create/Delete' )
        string( name: 'ImageName', description: 'Name of Docker Image Build', defaultValue: 'Java' )
        string( name: 'ImageTag', description: 'Image Tag for the Docker Image', defaultValue: 'v1' )
        string( name: 'DockerUser', description: 'DockerHub User Account', defaultValue: 'zohera27' )

    }

    stages{

        stage('Git Checkout') {

         when { expression { params.action == 'Create' } }

            steps{

                script{

                    gitcheckout(
                        branch: "main",
                        url: "https://github.com/zohera27/kubernetes-configmap-reload.git"
                    )
                }
            }
        }


    }


}