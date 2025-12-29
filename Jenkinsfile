pipeline {
  agent any

  environment {
    // SonarQube project key you created in SonarQube
    SONAR_PROJECT_KEY = "myfirstpipeline"
  }

  stages {
    stage('Build') {
      steps {
        echo "Version A - local change"
        echo "Version B - remote change"
      }
    }

    stage('SAST - SonarQube') {
      steps {
        script {
          // This name must match what you set in Jenkins -> Manage Jenkins -> Tools
          def scannerHome = tool 'SonarScanner'
          withSonarQubeEnv('SonarQube') {
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
