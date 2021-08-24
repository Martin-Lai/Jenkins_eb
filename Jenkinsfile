pipeline {
  agent any
  options {
      disableConcurrentBuilds()
  }
  environment {
    dockerImage = ''
    IMAGE_NAME = 'stock-backend'
    DOCKER_REG = 'https://052888887055.dkr.ecr.ap-southeast-1.amazonaws.com'
    AWS_REGION = 'ap-southeast-1'
    AWS_CREDENTIAL_ID = 'aws.ecr.key'

    APP_NAME = "stock-backend"
    APP_BUCKET = "stock-backend-eb-deployment"
  }
  stages {
    stage('Install Package') {
      steps {
          echo 'Install package'
      }
    }
    stage('Run Tests') {
      steps {
          echo 'Run Tests'
      }
    }
    stage('log env') {
      steps {
        echo "${env}"
      }
    }
    stage('Build') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:${BRANCH_NAME}-${BUILD_ID}")
        }
      }
    }
    stage('Push Image') {
      steps {
        script {
          docker.withRegistry("${DOCKER_REG}", "ecr:${AWS_REGION}:${AWS_CREDENTIAL_ID}" ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        step([$class: 'AWSEBDeploymentBuilder', applicationName: "${APP_NAME}", awsRegion: "${AWS_REGION}", bucketName: "${APP_BUCKET}", checkHealth: false, credentialId: "${AWS_CREDENTIAL_ID}", environmentName: "Stockbackend-${BRANCH_NAME}", keyPrefix: '', includes: 'Dockerfile, index.html, nginx.conf', maxAttempts: 10, rootObject: '', sleepTime: 20, versionDescriptionFormat: '', versionLabelFormat: '${BRANCH_NAME}-${BUILD_ID}', zeroDowntime: false])
      }
    }
  }
}
