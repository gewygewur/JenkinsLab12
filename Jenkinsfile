pipeline {
    agent any

    // Environment variables accessible in all stages
    environment {
        APP_NAME = 'MyDemoApp'
        BUILD_DIR = 'target'
    }

    // Parameters for dynamic builds
    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Execute test stage?')
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Environment to deploy (dev/staging/prod)')
    }

    // Tools to use (must match names in Jenkins Global Tool Configuration)
    tools {
        maven 'Maven 3.9.4'   // Example: name of Maven configured in Jenkins
        jdk 'JDK 17'          // Example: name of JDK configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${APP_NAME}..."
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${APP_NAME} to ${params.DEPLOY_ENV} environment..."
                bat "copy %BUILD_DIR%\\*.jar C:\\Temp\\${APP_NAME}-${params.DEPLOY_ENV}.jar"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs!'
        }
        always {
            echo "Build finished at ${new Date()}"
        }
    }
}
