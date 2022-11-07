pipeline{
    agent any
    
    tools{
        go 'v1.19.3'
    }
    
    environment{
        CLEAN_WORKSPACE = 'false'
    }
    
    
    stages{
        
        stage("clean workspace"){
             when {
                 environment name: 'CLEAN_WORKSPACE', value: 'true' 
             }
             steps{
                 echo "cleaning workspace"
                 cleanWs(deleteDirs: true)
             }
        }
        
        stage("git"){
            steps{
                echo '1. Cloning github repo, master branch'
                git 'https://github.com/CatalinPlesu/gitea_for_idweb'
            }    
        }
        
        stage('dependencies'){
            speps{
                echo '2. Getting all dependencies'
                sh 'go get -u ./..'
            }
        }
        
        stage("build"){
            steps{
                sh 'TAGS="bindata" make build'
            }
        }
        
        stage('test backend'){
            steps{
                sh 'go test $PWD/modules/emoji'
            }
        }
        
        stage('test frontend'){
            steps{
                sh 'make test-frontend'
            }
        }
    }
    
}
