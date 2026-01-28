pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/rohanlucifer/sonar-scanning-examples-devops.git'
      }
    }

    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('local-sonar') {
          sh '''
            sonar-scanner \
              -Dsonar.projectKey=sonar-demo \
              -Dsonar.sources=. \
              -Dsonar.host.url=http://localhost:9000
          '''
        }
      }
    }

    stage('Quality Gate') {
      steps {
        timeout(time: 3, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
