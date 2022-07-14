properties([disableConcurrentBuilds()])
pipeline {
  agent any
  tools {
    nodejs 'node_14_15_0'
  }
  stages {
    stage('SCM') {
        steps {
            checkout scm
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
    stage("Quality Gate") {
      steps {
          timeout(time: 1, unit: 'HOURS') {
              // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
              // true = set pipeline to UNSTABLE, false = don't
              waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}  