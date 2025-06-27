pipeline {
  agent any

  stages {
    stage('Clean Workspace') {
      steps {
        deleteDir() // 💥 Clears the old workspace
      }
    }

    stage('Checkout') {
      steps {
        echo "📥 Pulling latest code"
        checkout scm
      }
    }

    stage('Build & Deploy Containers') {
      steps {
        echo "🐳 Building Docker containers..."
        sh 'docker compose down'
        sh 'docker compose build'
        sh 'docker compose up -d'
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

