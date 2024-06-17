pipeline {
  agent any

  environment {
    DOCKERIMAGE = "pipeline-hello-world"
    HOME = "."
  }

  stages {
    stage('Limpiar Contenedores') {
      steps {
        script {
          sh '''
          CONTAINERS=$(docker ps -q)
          if [ ! -z "$CONTAINERS" ]; then
              docker stop $CONTAINERS
          else
              echo "No hay contenedores en ejecución."
          fi
          '''
        }
      }
    }

    stage('Build') {
      steps {
        script {
          docker.build(DOCKERIMAGE)
        }
      }
    }

    stage('Test') {
      steps {
        script {
          docker.image(DOCKERIMAGE).inside {
            sh 'npm install'
            sh 'npm test'
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        script {
          docker.image(DOCKERIMAGE).run('-d --restart unless-stopped -p 3000:3000')
        }
      }
    }
  }
}
