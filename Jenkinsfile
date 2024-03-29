pipeline {

  environment {
    registry = 'bran12v/jenkins_example'
    registryCredentials = 'docker'
    cluster_name = 'skillstorm'
    namespace = 'bran12v'
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

  stage('Kubernetes') {
    steps {
      withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
        sh "kubectl config use-context arn:aws:eks:us-east-1:797918408294:cluster/skillstorm"
        sh "aws eks update-kubeconfig --region us-east-1 --name ${cluster_name}"
        script{
          try{
            sh "kubectl create namespace ${namespace}"
          }catch (Exception e) {
              echo "Exception handled"
          }
        }
        sh "kubectl apply -f deployment.yaml -n ${namespace}"
        sh "kubectl -n ${namespace} rollout restart deployment flaskcontainer"
      }
    }
  }

  }
}