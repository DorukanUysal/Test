 pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""
PATH = "C:\\Users\\doruk\\AppData\\Local\\Programs\\Python\\Python311\\Scripts;${env.PATH}"
PATH = "C:\\Users\\doruk\\AppData\\Local\\Programs\\Python\\Python311\\Scripts\\semgrep;${env.PATH}"


        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"
    }
    stages {
      stage('Semgrep-Scan') {
          steps {
            bat 'pip3 install semgrep'
            bat 'semgrep ci'
          }
      }
    }
  }
