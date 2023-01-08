/* Jenkinsfile (Declarative Pipeline) */
pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    /* Grabbing credentials that we specified in Jenkins Controller:
        Dashboard > Manage Jenkins > Credentials > System > Global credentials (unrestricted) 
        (used PAT from DockerHub) */
    DOCKERHUB_CREDENTIALS = credentials('darinpope-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t darinpope/dp-alpine:latest .'
      }
    }
    stage('Login') {
      steps {
        /* This will cause password to be stored in plaintext in $HOME/.docker/config.json
        Think about using a credential helper if this is a problem
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store */
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push darinpope/dp-alpine:latest'
      }
    }
  }
  post {
    always {
        /* Logout no matter what, to purge the password from the $HOME/.docker/config.json file */
      sh 'docker logout'
    }
  }
}
