pipeline {
  agent any

  environment {
    SONAR_PROJECT_KEY = "myfirstpipeline"
    ZAP_TARGET        = "http://host.docker.internal:3000"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        echo "Build stage (your real build commands go here)"
      }
    }

    stage('SAST - SonarQube') {
      steps {
        script {
          def scannerHome = tool 'SonarScanner'   // must match Jenkins -> Tools name
          withSonarQubeEnv('SonarQube') {         // must match Jenkins -> System Sonar server name
            bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=%SONAR_PROJECT_KEY% -Dsonar.sources=."
          }
        }
      }
    }

    stage('Quality Gate') {
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }

    
  }
}
