@Library('Jenkins_Shared_Library') _

pipeline {

    agents any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the Action Item Crate/Delete' )
        string( name: 'ImageName', description: 'Docker Build Image', defaultValue: 'Java' )
        string( name: 'ImageTag', description: 'Docker ImageTag', defaultValue: 'v1' )
        string( name: 'Dockeruser', description: 'DockerHub user Account', defaultValue: 'zohera27' )
    }

    stages {

        stage('GIT Checkout') {

             when { expression {params.action == 'Create' } }

                steps {

                    script {         

                        gitcheckout(

                            branch: 'main',
                            url: 'https://github.com/zohera27/kubernetes-configmap-reload.git'
                        )
                    }    
                }         
        }
    }

}