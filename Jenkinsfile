pipeline {
  agent any
//   { label 'ecs-agent' }
//         options {
//             disableConcurrentBuilds()
//         }
//         environment {
//             region = 'eu-west-1'
//         }
    stages {
      stage('Checkout') {
    GIT_BRANCH_LOCAL = sh (
        script: "echo $Branch | sed -e 's|origin/||g'",
        returnStdout: true
    ).trim()
    echo "Git branch: ${GIT_BRANCH_LOCAL}"
      }
        stage("create lambda zip based on tag") {
            steps {
              echo "${env.GIT_BRANCH}"
              script {
            if ("${env.GIT_BRANCH}"== "origin/main") {
                ENV = 'prod'
               def sourceFile = "fetch_from_s3/prod.env"

if (fileExists(file: sourceFile)) {
    def newFile = "fetch_from_s3/.env"

    writeFile(file: newFile, encoding: "UTF-8", text: readFile(file: sourceFile, encoding: "UTF-8"))
}
            } else {
                ENV = 'dev'
//               fileOperations {
//             fileRenameOperation(destination: 'fetch_from_s3/.env', source: 'fetch_from_s3/dev.env')
//         }
              def sourceFile = "fetch_from_s3/dev.env"

if (fileExists(file: sourceFile)) {
    def newFile = "fetch_from_s3/.env"

    writeFile(file: newFile, encoding: "UTF-8", text: readFile(file: sourceFile, encoding: "UTF-8"))
}
            }
              }
                script {
                    DIR_SIZE = sh(returnStdout: true, script: "git describe --tags `git rev-list --tags --max-count=1` ")
                }
              script {
                if ("${DIR_SIZE}".contains("pipeline")) {
                    script{
                        zip archive: true, dir: 'pipeline_processor', glob: '', zipFile: 'PipelineProcessor.zip'
                        ZIP_FILE_NAME = 'PipelineProcessor.zip'
                        LAMBDA_NAME = 'smartevents-pipeline_processor-lambda'
                    }
                  echo "${ZIP_FILE_NAME}"
                  echo "${LAMBDA_NAME}"
                } else if ("${DIR_SIZE}".contains("s3lambda")) {
                    script{
                        zip archive: true, dir: 'fetch_from_s3', glob: '', zipFile: 'FetchFileS3b.zip'
                        ZIP_FILE_NAME = 'FetchFileS3b.zip'
                        LAMBDA_NAME = 'smartevents-fetchfroms3-lambda'
                    }
                   echo "${ZIP_FILE_NAME}"
                  echo "${LAMBDA_NAME}"
                }
                }
            }
        }
        stage ('deploy lambda based on branch') {
          steps{
            script {
            if ("${env.GIT_BRANCH}".contains("main")) {
                ENV = 'prod'
            } else {
                ENV = 'dev'
            }
            echo "Deploying ${LAMBDA_NAME} to ${ENV}"
            echo "aws lambda update-function-code --function-name ${LAMBDA_NAME}-${ENV} --zip-file fileb://${ZIP_FILE_NAME}"
        }
       }
      }    
   }
}
