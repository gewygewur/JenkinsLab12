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

    // Tools to use
    tools {
        maven 'Maven 3.9.4'  // Name must match the Maven tool in Jenkins configuration
        jdk 'JDK 17'          // Name must match the JDK tool in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${APP_NAME}..."
                sh 'mvn clean install'  // On Windows use: bat 'mvn clean install'
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                echo 'Running tests...'
                sh 'mvn test'  // On Windows: bat 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${APP_NAME} to ${params.DEPLOY_ENV} environment..."
                // Example deploy command:
                sh "cp ${BUILD_DIR}/*.jar /tmp/${APP_NAME}-${params.DEPLOY_ENV}.jar"
                // On Windows: bat "copy ${BUILD_DIR}\\*.jar C:\\Temp\\${APP_NAME}-${params.DEPLOY_ENV}.jar"
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
