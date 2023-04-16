@Library('Jenkins_Shared_Library') _

pipeline{

    agent any

    environment{

        SONARQUBE_SERVERURL = 'http://192.168.2.104:9000'
        SONARQUBE_PROJECT_KEY = 'com.minikube.sample:kubernetes-configmap-reload'
        SONARQUBE_TOKEN = credentials('sonar-api')
    }

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the Action Create/Delete' )
        string( name: 'ImageName', description: 'ImageName for Docker Build', defaultValue: 'Java')
        string( name: 'ImageTag', description: 'ImageTag Version for Docker Build', defaultValue: 'v1')
        string( name: 'DockerHub', description: 'DockerHub User Account', defaultValue: 'zohera27')

    }

    stages{

        stage('GIT CheckOut'){

         when { expression {params.action == 'Create'} }

            steps{

                script{

                    gitcheckout(

                        branch: "main",
                        url: "https://github.com/zohera27/kubernetes-configmap-reload.git"

                    )


                }



            }


        }

        stage('Maven Unit Testing'){

         when { expression {params.action == 'Create'} }

            steps{

                script{

                    maven.mvntest()
                }
            }

        }

        stage('Maven Integration Test'){

         when { expression { params.action == 'Create' } }    

            steps{

                script{

                    maven.mvnverify()
                }
            }
        }

        stage("SonarQube Static Code Analysis"){

         when { expression {params.action == 'Create' } }

            steps{

                script{

                    def SonarQUbeCredentialId = 'sonar-api'
                    maven.codeanalysis(SonarQUbeCredentialId)
                }
            }             
                     
        }

        stage('Delete SonarQube project') {
         
         when { expression {params.action == 'Delete' } }
            
            steps {

                script {

                    def SonarQUbeCredentialId = 'sonar-api'
                    sh "curl -u ${SONARQUBE_TOKEN} -X POST ${SONARQUBE_SERVER_URL}/api/projects/delete?project=${SONARQUBE_PROJECT_KEY}"
                    maven.codeclean(SonarQUbeCredentialId)
                }
            
            }
             
        }
    }

}