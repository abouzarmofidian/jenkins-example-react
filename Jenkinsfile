pipeline {
  agent any
  environment {
    docker-hub = credentials('docker-hub')
    IMAGE_NAME = 'amofidian69/jenkins-react-example'
    IMAGE_TAG = 'latest'
    APP_NAME = 'jenkins-example-react'
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $docker-hub | docker login -u --username=_ --password-stdin registry.heroku.com'
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
