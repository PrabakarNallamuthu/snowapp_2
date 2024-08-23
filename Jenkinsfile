pipeline {
  agent any
  environment {
    APPSYSID = '0132af27c3cc1250a28cfeac050131d5'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = '18be2029-2e62-4070-8828-dbb3aa39f0f0'
    DEVENV = 'https://dev263447.service-now.com/'
    TESTENV = 'https://dev196634.service-now.com/'
    PRODENV = 'https://dev196634.service-now.com/'
    // TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        // snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
