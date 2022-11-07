pipeline{
    agent any
    
    tools{
        go 'v1.19.3'
    }
    
    environment{
        CLEAN_WORKSPACE = 'false'
        ON_SUCCESS_SEND_EMAIL = 'true'
        ON_FAILURE_SEND_EMAIL = 'true'
        TESTING_FRONTEND = 'false'
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
        
        // stage("git"){
        //     steps{
        //         echo '1. Cloning github repo, master branch'
        //         git 'https://github.com/CatalinPlesu/gitea_for_idweb'
        //     }    
        // }

        // stage('dependencies'){
        //     steps{
        //         echo '2. Getting all dependencies'
        //         sh 'go get -u ./..'
        //     }
        // }
        
        // stage("build"){
        //     steps{
        //         sh 'TAGS="bindata" make build'
        //     }
        // }
        
        // stage('test backend'){
        //     steps{
        //         sh 'go test $PWD/modules/emoji'
        //     }
        // }
        
        // stage('test frontend'){
        //      when {
        //          environment name: 'TESTING_FRONTEND', value: 'true' 
        //      }
        //     steps{
        //         sh 'make test-frontend'
        //     }
        // }
    }

    post {
        success {
            emailext body: 'A Test EMail', subject: 'Success', to 'catalinsfake@gmail.com'
        }
        failure {
            emailext body: 'A Test EMail', subject: 'Failure', to 'catalinsfake@gmail.com'
        }
    }

}
