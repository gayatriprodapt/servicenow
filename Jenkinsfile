pipeline {
  agent any
  environment {
    APPSYSID = '21b697da87e52150ab42fe29cebb35f4'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = '4a37b613-0182-45c9-b0d7-d829bd88d3f6'
    DEVENV = 'https://ven05366.service-now.com/'
    TESTENV = 'https://dev121163.service-now.com/'
    PRODENV = 'https://dev75867.service-now.com/'
    TESTSUITEID = 'ac9fcb900b20220050192f15d6673ad5'
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
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
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
