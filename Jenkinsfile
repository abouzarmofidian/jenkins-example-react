pipeline {
  agent any
  environment {
    docker_hub = credentials('docker-hub')
    IMAGE_NAME = 'amofidian69/test'
    IMAGE_TAG = '3'
    APP_NAME = 'jenkins-example-react'
  }
  stages {
    stage('Build') {
      steps {
        echo 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $docker_hub | docker login -u --username=_ --password-stdin'
      }
    }
    stage('Push to Docker Hub') {
      steps {
        sh '''
          docker push $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }
    stage('Release the image') {
      steps {
        sh '''
          docker run -d --name react-example -port 3000:3000 $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
