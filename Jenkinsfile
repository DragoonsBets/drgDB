pipeline {
  agent {
    label "jenkins-nodejs"
  }
  environment {
    ORG = 'dragoonsbets'
    APP_NAME = 'drgdb'
    CHARTMUSEUM_CREDS = credentials('jenkins-x-chartmuseum')
  }
  stages {
    stage('Build Release') {
      when {
        branch 'master'
      }
      steps {
        container('nodejs') {

          // ensure we're not on a detached head
          sh "git checkout master"
          sh "git config --global credential.helper store"
          sh "jx step git credentials"

          // so we can retrieve the version in later steps
          sh "echo \$(jx-release-version) > VERSION"
          // Let's create tag in Git
          sh "jx step tag --version \$(cat VERSION)"
        }
      }
    }
    stage('Promote to Environments') {
    when {
      branch 'master'
    }
    steps {
      container('nodejs') {
        dir('./charts/drgdb') {
          sh "jx step changelog --version v\$(cat ../../VERSION)"

          // release the helm chart
          sh "jx step helm release"

          // promote through all 'Auto' promotion Environments
          // sh "jx promote -b --all-auto --timeout 1h --version \$(cat ../../VERSION)"
        }
      }
    }
    }
  }
  post {
        always {
          cleanWs()
        }
  }
}
