pipeline {

  environment {
    registry = 'bran12v/jenkins_example'
    registryCredentials = 'docker'
    cluster_name = 'skillstorm'
  }

  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/bran12v/flask_app_jenkins.git', branch: 'main')
      }
    }

    stage('Build Stage') {
      steps {
        script {
          dockerImage = docker.build(registry)
        }
      }
    }

    stage('Deployment Stage') {
      steps {
        script {
          docker.withRegistry('', registryCredentials) {
            dockerImage.push()
          }
        }
      }
    }

  }
}