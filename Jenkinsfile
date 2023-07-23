pipeline {
  agent any
    environment {

      PATH = "C:\\Users\\doruk\\AppData\\Local\\Programs\\Python\\Python311\\Scripts;${env.PATH}"
      SEMGREP_RULES = "p/default" 
      SEMGREP_BRANCH = "${GIT_BRANCH}"

      // Uncomment the following line to scan changed 
      // files in PRs or MRs (diff-aware scanning): 
      // SEMGREP_BASELINE_REF = "main"
    }
    stages {
      stage('Semgrep-Scan') {
        steps {
          bat 'pip install semgrep'
          bat 'semgrep ci'
      }
    }
  }
}
