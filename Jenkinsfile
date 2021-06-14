pipeline {
  agent { label 'macos' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKER_HUB_CREDS = credentials('darinpope-docker-hub')
    AWS_CREDS = credentials('darinpope-aws-creds')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker context use default'
        sh 'docker compose build'
        sh 'docker compose push'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker context use darinpope-ecs-context'
        sh 'docker compose up'
        sh 'docker compose ps --format json'
      }
    }
  }
}