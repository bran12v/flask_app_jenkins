pipeline {
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

    stage('Build') {
      steps {
        sh 'docker build -t bran12v/jenkins_example .'
      }
    }

    stage('Docker Login') {
      steps {
        sh 'docker login -u bran12v -p dckr_pat_F3k60vxZai73R-P2OElFPd2kcXY'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push bran12v/jenkins_example'
      }
    }

  }
}