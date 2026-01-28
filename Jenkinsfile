pipeline {
  agent any

  environment {
    // Path to SonarScanner installed in Jenkins global tools
    SONAR_SCANNER_HOME = tool 'SonarScanner'
  }

  stages {

    stage('Checkout') {
      steps {
        // Checkout the Sonar repo
        git(
          url: 'https://github.com/rohanlucifer/sonar-scanning-examples-devops.git',
          credentialsId: 'github-secret' // if needed for private repo
        )
      }
    }

    stage('SonarQube Scan') {
      steps {
        // Injects Sonar environment variables (server URL, token)
        withSonarQubeEnv('local-sonar') {
          sh '''
            ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
              -Dsonar.projectKey=sonar-demo \
              -Dsonar.projectName="Sonar Scanning Examples" \
              -Dsonar.sources=. \
              -Dsonar.host.url=$SONAR_HOST_URL \
              -Dsonar.login=$SONAR_AUTH_TOKEN
          '''
        }
      }
    }

    stage('Quality Gate') {
      steps {
        // Wait for SonarQube quality gate result
        timeout(time: 3, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
