@Library('Jenkins_Shared_Library') _

def mavenhome = "/opt/apache-maven-3.9.1"
def toolchain = "/opt/apache-maven-3.9.1/conf/toolchains.xml"

pipeline{

    agent any
    /*
    environment{

        SONARQUBE_SERVER_URL = 'http://192.168.2.104:9000'
        SONARQUBE_PROJECT_KEY = 'com.minikube.sample:kubernetes-configmap-reload'
        SONARQUBE_TOKEN = credentials('sonar-api')
        
    }
    */
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

                    maven.mvntest(mavenhome, toolchain)
                }
            }

        }

        stage('Maven Integration Test'){

         when { expression { params.action == 'Create' } }    

            steps{

                script{

                    maven.mvnverify(mavenhome, toolchain)
                }
            }
        }

        stage("SonarQube Static Code Analysis"){

         when { expression {params.action == 'Create' } }

            steps{

                script{

                    def SonarQUbeCredentialId = 'sonar-api'
                    maven.codeanalysis(SonarQUbeCredentialId, mavenhome, toolchain)
                }
            }             
                     
        }

        stage('Delete SonarQube project') {
         
         when { expression {params.action == 'Delete' } }
            
            steps {

                script {

                    def SonarQUbeCredentialId = 'sonar-api'
                    sh "curl -u ${SONARQUBE_TOKEN} -X POST ${SONARQUBE_SERVER_URL}/api/projects/delete?project=${SONARQUBE_PROJECT_KEY}"
                    maven.codeclean(SonarQUbeCredentialId, mavenhome, toolchain)
                }
            
            }
             
        }
    }

    post {
        always {
            script {
                // Save the project key to a global variable
                def projectKey = ''
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                    projectKey = sh script: "${mavenhome}/bin/mvn -Dmaven.toolchains.file=${toolchain} -Djdk-version=1.8 -B -e -T 1C sonar:sonar -Dsonar.projectKey=${env.SONARQUBE_PROJECT_KEY} -Dsonar.host.url=${env.SONARQUBE_HOST_URL} -Dsonar.login=${env.SONARQUBE_TOKEN} -Dsonar.analysis.mode=publish -Dsonar.github.oauth=${env.GITHUB_OAUTH} -s ${env.MAVEN_SETTINGS_XML} | grep 'ANALYSIS SUCCESSFUL, you can browse ' | awk \"{print \$9}\"", returnStdout: true
                }
                // Print the project key for verification
                println "SonarQube project key: ${projectKey}"
            }
        }
    }
}