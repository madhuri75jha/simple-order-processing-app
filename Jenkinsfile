pipeline {
  agent any
  environment {
    MAVEN_VERSION = "3.9.12"
    MAVEN_HOME="/opt/maven"
    PATH = "/opt/maven/bin:${env.PATH}"
  stages {
    stage('Install Maven'){
      steps {
        sh '''
        echo "Installing Maven ${MAVEN_VERSION}"
        sudo rm -rf /opt/maven
        cd /tmp
        wget https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz
        tar -xzf apache-maven-${MAVEN_VERSION}-bin.tar.gz
        sudo mv apache-maven-${MAVEN_VERSION} /opt/maven
        sudo chown -R ubuntu:ubuntu /opt/maven
        /opt/maven/bin/mvn -version
        '''
            }
        }
    
    stage('Checkout Code') {
      steps{
        git branch: 'main', url: 'https://github.com/madhuri75jha/simple-order-processing-app.git'
      }
    }

    stage('Build with Maven') {
      steps{
        sh 'mvn clean package -Dmaven.test.failure.ignore=true'
      }
    }
  }  
  post {
    success {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
    }
    failure {
      echo 'Build Failed'
    }
  }
        
}
