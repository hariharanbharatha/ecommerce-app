pipeline {
  agent any

  environment {
    DOCKER_COMPOSE_VERSION = '1.29.2'
  }

  stages {
    stage('Checkout') {
      steps {
        echo "📥 Pulling latest code"
        checkout scm
      }
    }

    stage('Install Docker Compose (if missing)') {
      steps {
        sh '''
        if ! command -v docker-compose &> /dev/null
        then
          echo "Installing docker-compose..."
          sudo curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
          sudo chmod +x /usr/local/bin/docker-compose
        fi
        '''
      }
    }

    stage('Build & Deploy Containers') {
      steps {
        echo "🐳 Building Docker containers..."
        sh 'docker-compose down'
        sh 'docker-compose build'
        sh 'docker-compose up -d'
      }
    }

    stage('Verify Running Containers') {
      steps {
        sh 'docker ps'
      }
    }
  }

  post {
    always {
      echo "✅ Pipeline completed"
    }
    failure {
      echo "❌ Build failed. Please check logs."
    }
  }
}

