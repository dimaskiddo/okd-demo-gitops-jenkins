pipeline {
  triggers {
    pollSCM ('H/3 * * * *')
  }
  options {
    disableConcurrentBuilds()
  }
  agent {
    node {
      label 'nodejs'
    }
  }
  stages {
    stage ('Validate configuration resources') {
      steps {
        sh 'oc apply --dry-run --validate -k config'
      }
    }
    stage ('Apply resources') {
      when {
        branch 'master'
      }     
      steps {
        sh 'oc apply -k config'
        sh './wait_oauth.sh'
      }
    }
    stage ('Verify test user') {
      when {
        branch 'master'
      }
      steps {
        sh 'oc login -u testuser -p redhat123 --insecure-skip-tls-verify https://api.openshift.pod03.io:6443'
        sh 'oc new-project test-testuser || true'
      }
    }
  }
}
