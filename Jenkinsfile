@Library('Jenkins_Shared_Library') _

def mavenhome = '/opt/apache-maven-3.9.1/bin'
def toolchain = '/opt/apache-maven-3.9.1/conf'
def SONARQUBE_TOKEN = credentials('sonar-api')
def SONARQUBE_SERVER_URL = 'http://192.168.2.104:9000'
def PROJECT_KEY = 'com.minikube.sample:kubernetes-configmap-reload'

pipeline {

    agent any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the Action Create/Delete' )
        string( name: 'ImageName', description: 'Docker Build Image', defaultValue: 'Java' )
        string( name: 'ImageTag', description: 'Docker ImageTag Version', defaultValue:'v1' )
        string( name: 'DockerHub', description: 'DockerHub User Account', defaultValue:'zohera27' )
    }

    stages {

        stage('GIT Checkout') {

         when { expression { params.action == 'Create' } }   
            
            steps {

                script {
            
                    gitcheckout(

                        branch: 'main',
                        url: 'https://github.com/zohera27/kubernetes-configmap-reload.git'
                    )
                }    
            }    
        }

        
    
        stage('Maven Test') {

         when { expression { params.action == 'Create' } }   
            
            steps {

                script {
            
                    maven.mvntest(mavenhome, toolchain)

                }    
            }    
        }
    
        stage('Maven UnitTesting') {

         when { expression { params.action == 'Create' } }   
            
            steps {

                script {
            
                    maven.mvnverify(mavenhome, toolchain)

                }    
            }    
        }

        stage('SonarQube StaticCode Analysis'){

         when { expression { params.action == 'Create' } }

            steps{

                script{

                    def SonarQubeCredentialId = 'sonar-api'
                    sonarqube.staticcode(SonarQubeCredentialId, mavenhome, toolchain)
                }
            }
        }

        stage('SonarQube Quality Gate Status') {

         when { expression { params.action == 'Create' } }
         
            steps{

                script{

                    def SonarQubeCredentialId = 'sonar-api'
                    sonarqube.qualitygate(SonarQubeCredentialId)
                }
            }
        }

        stage('Docker Image Build'){

         when { expression { params.action == 'Create' } }

            steps{

                script{

                    docker.build("${params.ImageName}", "${params.ImageTag}", "${params.DockerHub}")
                }
            }
        }        

    }    

}