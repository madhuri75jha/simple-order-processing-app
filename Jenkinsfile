pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git(branch: 'main', url: 'https://github.com/madhuri75jha/simple-order-processing-app.git')
        sh 'echo "Git Checkout Completed"'
      }
    }

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

    stage('Deploy') {
      steps {
        sh '''
        echo "Deploying application..."
        mvn package -Dmaven.test.failure.ignore=true
        '''
      }
    }

  }
  tools {
    maven 'maven_3'
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