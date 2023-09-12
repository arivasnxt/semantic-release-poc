#!groovy
pipeline {
  parameters {
    booleanParam(
      name: 'RELEASE_BETA',
      defaultValue: 'false',
      description: 'Generate a beta version if the branch matches any of: beta, feature/*, task/*, hotfix/*'
    )
  }

  agent {
    label 'nodejs'
  }

  stages {
    stage('Build') {
      steps {
        sh 'node --version'
        sh 'npm --version'
        sh 'npm ci'
      }
    }

    stage('Npm publish') {
      when {
        anyOf {
          expression { params.RELEASE_BETA }
          expression { env.BRANCH_NAME.equals('main') }
          expression { env.BRANCH_NAME.equals('beta') }
          expression { env.BRANCH_NAME =~ /(\d+)\.(\d+)\.(\d+)/ }
        }
      }
      steps {
        sshagent(['jenkins.git']) {
          script {
            echo 'released!!!'
          }
        }
      }
    }
  }
}
