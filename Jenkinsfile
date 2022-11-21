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
        EMAIL_TO = 'catalinsfake@gmail.com'
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
            steps{
                echo '2. Getting all dependencies'
                sh 'make deps'
                sh 'TAGS="bindata" make build'
            }
        }
        
         stage('test backend'){
            steps{
                sh 'go test $PWD/modules/emoji -v 2>&1 ./... | ../../go/bin/go-junit-report -set-exit-code > report.xml'
                // sh 'go test -v 2>&1 ./... | ../../go/bin/go-junit-report -set-exit-code > report.xml || echo "0"'
            }
            post {
                always{
                    junit 'report.xml'
                }
            }
        }
        
        stage('test frontend'){
            steps{
                echo "TESTING_FRONTEND: $TESTING_FRONTEND"
                script{
                    if (TESTING_FRONTEND == true){
                        sh 'make test-frontend'
                    }
                }
            }
        }
        
    }

    post {
        success { 
            script{
                if (ON_SUCCESS_SEND_EMAIL == 'true'){
                      emailext body: 'JOB_NAME: ${JOB_NAME},\n BUILD_NUMBER: ${BUILD_NUMBER},\n BUILD_URL: ${BUILD_URL}', 
                        to: "${EMAIL_TO}", 
                       subject: 'Build succeeded in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
                        echo 'success'
                 }
            }
        }
        failure {
            script{
                if (ON_SUCCESS_SEND_EMAIL == 'true'){
                    emailext body: 'JOB_NAME: ${JOB_NAME},\n BUILD_NUMBER: ${BUILD_NUMBER},\n BUILD_URL: ${BUILD_URL}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
                    echo 'failure'
                }
            }
        }
    }
}


