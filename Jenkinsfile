pipeline {
  agent any

  environment {
    DOCKERIMAGE = "pipeline-hello-world"
    HOME = "."
  }

  stages {
    stage('Limpiar Contenedores') {
            steps {
                    sh 'docker stop $(docker ps -q)'
            }
        }
    stage ('Build') {
      steps {
        script {
          docker.build(DOCKERIMAGE)
        }
      }
    }

    stage ('Test') {
      steps {
        script {
          docker.image(DOCKERIMAGE).inside {
            sh 'npm install'
            sh 'npm test'
          }
        }
      }
    }
    stage ('Deploy') {
      steps {
        script {
          docker.image(DOCKERIMAGE).run('-d -p 3000:3000')
        }
      }
    }
  }
}