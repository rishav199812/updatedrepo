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
//       environment {
//                 scannerHome = D:\sonar-scanner-4.7.0.2747-windows
//             }
      steps {
        withSonarQubeEnv(installationName: 'sq1') { 
          //sh './mvnw clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
          sh "D:\sonar-scanner-4.7.0.2747-windows\bin\sonar-scanner -Dsonar.projectKey=simple-test-sonar  -Dsonar.sources=.  "
        }
      }
    }
  }
}
