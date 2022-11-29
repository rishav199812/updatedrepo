pipeline {
  agent { label 'ecs-agent' }
        options {
            disableConcurrentBuilds()
        }
        environment {
            region = 'eu-west-1'
        }
    stages {
        stage("create lambda zip based on tag") {
            steps {
                script {
                    DIR_SIZE = sh(returnStdout: true, script: "git describe --tags `git rev-list --tags --max-count=1` ")
                }
                if ("${DIR_SIZE}".contains("pipeline")) {
                    script{
                        zip archive: true, dir: 'pipeline_processor', glob: '', zipFile: 'PipelineProcessor.zip'
                        ZIP_FILE_NAME = 'PipelineProcessor.zip'
                        LAMBDA_NAME = 'smartevents-pipeline_processor-lambda'
                    }
                } else if ("${DIR_SIZE}".contains("s3lambda")) {
                    script{
                        zip archive: true, dir: 'fetch_from_s3', glob: '', zipFile: 'FetchFileS3.zip'
                        ZIP_FILE_NAME = 'FetchFileS3.zip'
                        LAMBDA_NAME = 'smartevents-fetchfroms3-lambda'
                    }
                }
            }
        }
        stage ('deploy lambda based on branch') {
            if ("${env.GIT_BRANCH}".contains("master")) {
                ENV = 'prod'
            } else {
                ENV = 'dev'
            }
            echo "Deploying ${LAMBDA_NAME} to ${ENV}"
            sh "aws lambda update-function-code --function-name ${LAMBDA_NAME}-${ENV} --zip-file fileb://${ZIP_FILE_NAME}"
        }
   }
}
