pipeline {
  agent any
  parameters {
    string(name: 'SLEEP_TIME', defaultValue: '5', description: 'Sleep time in seconds')
    string(name: 'APP_PORT', defaultValue: '6767', description: 'Application Port')
    string(name: 'BRANCH_NAME', defaultValue: 'test', description: 'Branch name')
  }
  stages {
    stage('Maven Version') {
      steps {
        sh 'echo Print Maven Version'
        sh 'mvn -version'
        sh "echo Sleep-Time - ${params.SLEEP_TIME}, PORT - ${params.APP_PORT}, Branch-Name - ${params.BRANCH_NAME}"
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
        sh """ java -jar target/hello-demo-*.jar --server.port=${params.APP_PORT} > /dev/null & """
      }
    }
    
    stage('Integration Testing') {
      steps {
        sh "sleep ${params.SLEEP_TIME}"
        sh "curl -s http://localhost:${params.APP_PORT}/hello"
      }
    }
  }
  tools {
    maven 'M399'
  }
}
