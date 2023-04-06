pipeline {
  agent {label 'docker'}
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          sudo docker version
          sudo docker info
          sudo docker compose version 
          sudo curl --version
          sudo jq --version
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'sudo docker system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
        sh 'sudo docker compose up -d --no-color --wait'
        sh 'sudo docker compose ps'
      }
    }
    stage('Run tests against the container') {
      steps {
        sh 'sudo curl http://localhost:3000/param?query=demo | jq'
      }
    }
  }
  post {
    always {
      sh 'sudo docker compose down --remove-orphans -v'
      sh 'sudo docker compose ps'
    }
  }
}
