pipeline {
  agent none
  options { skipDefaultCheckout() }
  stages {
    stage('Init'){
      agent {
        label "master"
      }
      steps {
        cleanWs()
        checkout scm
        stash name: 'repo', excludes: "git"
      }
    }
    stage('BUILD&DEPLOY'){
      agent {
        label "TL"
      }
      steps {
        cleanWs()
        unstash 'repo'
        script {
          docker.image("node:10.16.0-alpine").inside("-u root") {
            sh '''
            ls -al
            npm ci
            npm run build
            '''
          }
        }
      }
    }
  }
}
