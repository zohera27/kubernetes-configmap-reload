@Library('Jenkins_Shared_Library') _

pipeline{

    agent any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the Action Create/Delete' )
        string( name: 'ImageName', description: 'ImageName for Docker Build', defaultValue: 'Java')
        string( name: 'ImageTag', description: 'ImageTag Version for Docker Build', defaultValue: 'v1')
        string( name: 'DockerHub', description: 'DockerHub User Account', defaultValue: 'zohera27')

    }

    stages{

        stage('GIT CheckOut'){

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