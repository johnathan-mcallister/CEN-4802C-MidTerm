pipeline {
  agent any
  stages {
    stage('WSL + Docker Sanity') {
      steps {
        bat 'whoami'
        bat 'wsl -l -v'
        bat 'wsl docker version'
        bat 'wsl docker ps'
      }
    }
  }
}