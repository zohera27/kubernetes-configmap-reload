@Library('Jenkins_Shared_Library') _

def mavenhome = '/opt/apache-maven-3.9.1/bin'
def toolchain = '/opt/apache-maven-3.9.1/conf'

pipeline {

    agent any

    parameters{

        choice( name: 'action', choices: 'Create\nDelete', description: 'Choose the Action Create/Delete' )
        string( name: 'ImageName', description: 'Docker Build Image', defaultValue: 'Java' )
        string( name: 'ImageTag', description: 'Docker ImageTag Version', defaultValue:'v1' )
        string( name: 'DockerHub', description: 'DockerHub User Account', defaultValue:'zohera27' )
    }

    



}