pipeline {
  agent any

  environment {
    IMAGE_NAME = "cen4802c-midterm"
    CONTAINER_NAME = "cen4802c-midterm-app"
    APP_PORT = "8080" // change if your app uses a different port
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build + Test (Maven/JUnit)') {
      steps {
        bat 'mvn -B clean test package'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Docker Build (WSL)') {
      steps {
        bat 'wsl docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
        bat 'wsl docker image ls %IMAGE_NAME% --format "{{.Repository}}:{{.Tag}}"'
      }
    }

    stage('Deploy (Run Container)') {
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