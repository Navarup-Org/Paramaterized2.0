pipeline {
  agent any

  environment {
    SLEEP_TIME = params.SLEEP_TIME ?: '5'  // Default value if not set
    APP_PORT = params.APP_PORT ?: '8080'
    BRANCH_NAME = params.BRANCH_NAME ?: 'test'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'test', url: 'https://github.com/your-repo.git'
        sh 'echo Checked out test branch'
      }
    }

    stage('Maven Version') {
      steps {
        sh 'echo Print Maven Version'
        sh 'mvn -version'
        sh "echo Sleep-Time - ${SLEEP_TIME}, PORT - ${APP_PORT}, Branch-Name - ${BRANCH_NAME}"
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }
    
    stage('Local Deployment') {
      steps {
        sh """ java -jar target/hello-demo-*.jar > /dev/null & """
      }
    }
    
    stage('Integration Testing') {
      steps {
        sh "echo Sleeping for ${SLEEP_TIME} seconds..."
        sh "sleep ${SLEEP_TIME}"
        sh "curl -s http://localhost:${APP_PORT}/hello"
      }
    }
  }

  tools {
    maven 'M399'
  }
}
