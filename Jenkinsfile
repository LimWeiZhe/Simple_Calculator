pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        deleteDir() // Optional: Clear workspace
        git branch: 'main', url:'https://github.com/LimWeiZhe/Simple_Calculator.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        // Use 'bat' for Windows environment
        bat 'pip install -r requirements.txt'
      }
    }

    stage('Build') {
      steps {
        // Build steps here
        echo 'Running calculator script...'
        bat 'python calculator.py 1 10 20' // Passes the operation and numbers as argument
      }
    }

    stage('Test') {
      steps {
        // Test steps here
        script {
          // Continue to the next stage even if tests fail
          catchError(buildResult: 'UNSTABLE', stageResult:'FAILURE') {
            echo 'Running unit tests...'
            bat 'pytest --maxfail=1 --disable-warnings'
          }
        }
      }
    }

stage('Deploy to GitHub Pages') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        npm install -g --silent gh-pages@2.1.1
                        git config user.email "jenkins@example.com"
                        git config user.name "Jenkins"
                        gh-pages --dotfiles --message '[skip ci] Updates' --dist build
                    """
                }
            }
        }
    }
  
  post {
    success {
      echo 'Build, test, and deployment stages completed successfully.'
    }
    failure {
      echo 'One or more stages failed. Check the logs for details.'
    }
  }
}

