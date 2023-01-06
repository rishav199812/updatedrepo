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
      steps {
        withSonarQubeEnv(installationName: 'sq1') { 
          //sh './mvnw clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
          sh" ${SCANNER_HOME}}/bin/sonar-scanner \
                    -Dsonar.projectKey=simple_webapp \
                    -Dsonar.sources=. "
        }
      }
    }
  }
}
