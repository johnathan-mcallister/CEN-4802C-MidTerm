pipeline {
  agent any

  environment {
    IMAGE_NAME = "cen4802c-midterm"
    CONTAINER_NAME = "cen4802c-midterm-app"
    APP_PORT = "3000"
  }

  stages {

    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Node Version') {
      steps {
        bat 'node -v'
        bat 'npm -v'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm ci'
      }
    }

    stage('Docker Build') {
      steps {
        bat 'wsl docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
      }
    }

    stage('Deploy Container') {
      steps {
        bat '''
          wsl docker rm -f %CONTAINER_NAME% 2>nul || exit /b 0
          wsl docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:%APP_PORT% %IMAGE_NAME%:%BUILD_NUMBER%
        '''
      }
    }

    stage('Verify Running') {
      steps {
        bat 'wsl docker ps --filter "name=%CONTAINER_NAME%"'
      }
    }

  }
}