pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
      stage ('Hello') {
          steps {
              echo "Hello World"
          }
      }
    stage('Scan') {
      environment {
                scannerHome = tool 'sonar-scanner'
            }
      steps {
        withSonarQubeEnv('sq1') { 
          //env.PATH = "${tool 'sonar-scanner'}/bin:${env.PATH}"
          //sh './mvnw clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
          sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=simple-test-sonar  -Dsonar.sources=.  "
        }
      }
    }
  }
}
