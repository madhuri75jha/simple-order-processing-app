pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git(branch: 'main', url: 'https://github.com/madhuri75jha/simple-order-processing-app.git', poll: true, changelog: true)
        sh 'echo \'Git Checkout\''
      }
    }

    stage('Build & Test') {
      parallel {
        stage('Build') {
          steps {
            sh '''
            echo "Building application..."
            mvn clean compile
            '''
          }
        }

        stage('Test') {
          steps {
            sh '''
            echo "Running tests..."
            mvn test
            '''
          }
        }

      }
    }

    stage('Package') {
      steps {
        sh '''
        echo "Packaging artifact..."
        mvn package -Dmaven.test.failure.ignore=true
        '''
      }
    }

  }
  tools {
    maven 'maven_3'
  }
  environment {
    maven_3 = '3.9.9'
  }
  post {
    success {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
      echo 'Build Pipeline Completed Successfully'
    }

    failure {
      echo 'Build Failed'
    }

  }
}