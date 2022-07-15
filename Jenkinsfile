pipeline {
  agent any
  options {
      disableConcurrentBuilds()
  }
  tools {
    nodejs 'node_14_15_0'
  }
  stages {
    stage('SCM') {
        steps {
            checkout scm
        }
    }
    stage("Test and covarage") {
      sh '''
         yarn
         yarn jest --coverage
         '''
    }
    stage('SonarQube analysis') {
      steps {
        script {
          def scannerHome = tool 'sonar';
          withSonarQubeEnv('sonarqube') {
            sh "${tool("sonar")}/bin/sonar-scanner -Dsonar.projectKey=tradersclub_TCWeb \
                                                   -Dsonar.projectName=TCWeb \
                                                   -Dsonar.javascript.lcov.reportPaths=./coverage/lcov.info"
          }
        }
      }
    }
//    stage("Quality Gate") {
//      steps {
//          timeout(time: 10, unit: 'MINUTES') {
//              // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
//              // true = set pipeline to UNSTABLE, false = don't
//              waitForQualityGate abortPipeline: true
//        }
//      }
//    }
  }
}  