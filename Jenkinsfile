pipeline {
  agent any
    environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')

      // Uncomment the following line to scan changed 
      // files in PRs or MRs (diff-aware scanning): 
      // SEMGREP_BASELINE_REF = "main"

      // Troubleshooting:

      // Uncomment the following lines if Semgrep Cloud Platform > Findings Page does not create links
      // to the code that generated a finding or if you are not receiving PR or MR comments.
      // SEMGREP_JOB_URL = "${BUILD_URL}"
      // SEMGREP_COMMIT = "${GIT_COMMIT}"
      // SEMGREP_BRANCH = "${GIT_BRANCH}"
      // SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*).git$/, '$1')
      // SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
      // SEMGREP_PR_ID = "${env.CHANGE_ID}"
    }
    stages {
      stage('Semgrep-Scan') {
        steps {
          sh 'pip3 install semgrep'
          sh 'semgrep ci'
stage('Clone Repository') {
            steps {
                // GitHub reposunu Jenkins çalışma alanına klonlayın
                git 'https://github.com/KULLANICI_ADI/REPO_ADI.git'
            }
        }
        stage('OWASP ZAP Baseline Scan') {
            steps {
                // OWASP ZAP tarama komutunu burada çalıştırın
                sh 'owasp-zap-command -t https://github.com/KULLANICI_ADI/REPO_ADI.git -r report.html'
            }
        }
        stage('Publish Results') {
            steps {
                // OWASP ZAP raporlarını uygun bir şekilde yayınlayın
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, 
                            keepAll: true, reportDir: 'reports', reportFiles: 'report.html'])
            }
        }
      }
    }
  }
}
