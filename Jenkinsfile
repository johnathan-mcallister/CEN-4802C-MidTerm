pipeline {
  agent any

  environment {
    IMAGE_NAME = "cen4802c-midterm"
    CONTAINER_NAME = "cen4802c-midterm-app"
    APP_PORT = "3000"   // change if your app uses a different port
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Node + NPM Versions') {
      steps {
        bat 'node -v'
        bat 'npm -v'
      }
    }

    stage('Install Dependencies') {
      steps {
        // Uses package-lock.json for reproducible installs
        bat 'npm ci'
      }
    }

    stage('Test') {
      steps {
        // Only runs if you have a test script; otherwise it will fail.
        // If you don't have tests yet, see the "Allow no tests" option below.
        bat 'npm test'
      }
    }

    stage('Build (optional)') {
      steps {
        // Only if you have a build step (React/TS/etc). If not, you can remove this stage.
        bat 'npm run build'
      }
    }

    stage('Docker Build (WSL)') {
      steps {
        bat 'wsl docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
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