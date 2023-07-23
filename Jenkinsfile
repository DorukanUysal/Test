pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"

PATH = "C:\\Users\\doruk\\AppData\\Local\\Programs\\Python\\Python311\\Scripts;${env.PATH}"

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
