pipeline {
  enviornment {
    imagename = "teknofile/docker-test-image"
    registryCredential = 'teknofile-dockerhub'
    dockerImage = ''
  }

  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/teknofile/docker-image-test.git', branch: 'master', credentialsId: 'teknofile-github-user-token'])
      }
    }

    stage('Building Image') {
      steps {
        script {
          dockerImage = docker.build imagename
        }
      }
    }

    stage('Deploy Image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push("${BUILD_NUMBER}")
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Remove Unused docker image') {
      steps {
        sh "docker rmi $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:latest"
      }
    }
  }
}
