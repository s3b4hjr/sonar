pipeline {
  agent any
  options {
      disableConcurrentBuilds()
  }
  tools {
    nodejs 'node_14_17_0'
  }
  stages {
    stage('SCM') {
        steps {
            checkout scm
        }
    }
    stage("Test and covarage") {
      steps {
        withCredentials([string(credentialsId: 'NPM_TOKEN', variable: 'NPM_TOKEN_KEY')]) {
              sh "echo @tradersclub:registry=https://npm.pkg.github.com > .npmrc"
              sh "echo '//npm.pkg.github.com/:_authToken=${NPM_TOKEN_KEY}' >> .npmrc"
              sh 'echo engine-strict = true'            
          }          

        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh "yarn"
          sh "yarn jest --coverage --runInBand"
      }
    }
  }  
    stage('SonarQube analysis') {
      steps {
        script {
          def scannerHome = tool 'sonar';
          withSonarQubeEnv('sonarqube') {
            sh "${tool("sonar")}/bin/sonar-scanner -Dsonar.projectKey=tradersclub_TCWeb \
                                                   -Dsonar.projectName=TCWeb \
                                                   -Dsonar.javascript.lcov.reportPaths=./tests/coverage/lcov.info"
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
