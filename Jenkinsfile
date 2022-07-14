properties([disableConcurrentBuilds()])
pipeline {
  agent any
  tools {
    nodejs 'node_14_15_0'
  }
  options {
    // This is required if you want to clean before build
    skipDefaultCheckout(true)
  }
  stages {
    stage('SCM') {
        steps {
            git url: 'https://github.com/tradersclub/TCWeb.git'
        }
    }
    stage('SonarQube analysis') {
      steps {
        script {
          def scannerHome = tool 'sonar';
          withSonarQubeEnv('sonarqube') {
            sh "${tool("sonar")}/bin/sonar-scanner -Dsonar.projectKey=tradersclub_TCWeb -Dsonar.projectName=TCWeb"
          }
        }
      }
    }
//    stage("Quality gate") {
//      steps {
//        script {
//          def qualitygate = waitForQualityGate()
//          sleep(10)
//          if (qualitygate.status != "OK") {
//            waitForQualityGate abortPipeline: true
//          }
//        }
//      }
//    }
  }
}
