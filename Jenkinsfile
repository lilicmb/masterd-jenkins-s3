pipeline {
  agent any
  environment {
    AWS_DEFAULT_REGION = 'eu-central-1'      // Frankfurt
    S3_BUCKET = 'lilicmb-masterd-efm1'     // <-- CAMBIA con tu bucket
  }
  stages {
    stage('cli installation') {
      steps {
        sh 'mkdir -p /var/jenkins_home/workspace/temp'
        dir('/var/jenkins_home/workspace/temp') {
          sh 'curl -sSL "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"'
          sh 'unzip -o awscliv2.zip'
          sh './aws/install -b /tmp/aws-cli -i /var/jenkins_home/workspace/temp/aws-cli --update'
        }
      }
    }

    stage('push files') {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
          credentialsId: 'aws-credentials',
          usernameVariable: 'AWS_ACCESS_KEY_ID',
          passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          sh '/tmp/aws-cli/aws s3 cp index.html s3://$S3_BUCKET/index.html --region $AWS_DEFAULT_REGION'
        }
      }
    }
  }
}
