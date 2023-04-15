@Library('Jenkins_Shared_Library') _

pipeline{

    agent any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the option Create/Delete' )
        string( name: 'ImageName', description: 'Name of Docker Image Build', defaultname: 'Java' )
        string( name: 'ImageTag', description: 'Image Tag for the Docker Image', defaultname: 'v1' )
        string( name: 'DockerUser', description: 'DockerHub User Account', defaultname: 'zohera27' )

    }

    stages{

        stage('GIT CheckOut'){

         when { expression { params.action == 'Create' } }

            script{

                gitcheckout(

                    branch: "main",

                    url: "https://github.com/zohera27/kubernetes-configmap-reload.git"


                )


            }



        }


    }


}