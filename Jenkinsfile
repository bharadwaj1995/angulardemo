pipeline {
  agent any
  stages{
    stage ('checkout') {
      steps{
        checkout scm
      }
    }
    stage ('Build') {
      agent {
        docker { image 'node:10'}
      }
      steps {
        sh 'echo installing packages'
        sh 'npm install'
        sh 'npm install -g @angular/cli@8'
        sh 'echo Building Angular Project'
        sh 'ng build'

      }
    }
    stage ('push') {
      steps{
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '98b7f3ca-8b00-4cd3-a8fa-84aa613d2c85	', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws s3 ls'
        sh 'aws s3api create-bucket --bucket testing-sample-angular --region us-east-1'
        sh 'aws s3 sync ../angular_build@2/dist/ s3://testing-sample-angular/ --region us-east-1'
        }
        sh 'echo pushing success'
      }
    }

  }
}
