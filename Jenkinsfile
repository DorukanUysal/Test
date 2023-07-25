pipeline {
  agent any
  environment {
    // The following variable is required for a Semgrep Cloud Platform-connected scan

    // Uncomment the following line to scan changed 
    // files in PRs or MRs (diff-aware scanning): 
    SEMGREP_BASELINE_REF = "main"

    // Troubleshooting:

    // Uncomment the following lines if Semgrep Cloud Platform > Findings Page does not create links
    // to the code that generated a finding or if you are not receiving PR or MR comments.
    SEMGREP_JOB_URL = "${BUILD_URL}"
    SEMGREP_COMMIT = "${GIT_COMMIT}"
    SEMGREP_BRANCH = "${GIT_BRANCH}"
    SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*).git$/, '$1')
    SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
    SEMGREP_PR_ID = "${env.CHANGE_ID}"
  }
  stages {
    stage('Semgrep-Scan') {
      steps {
        script {
          def semgrepImage = 'returntocorp/semgrep'
          def semgrepCmd = "docker run \
              -e SEMGREP_REPO_URL=${SEMGREP_REPO_URL} \
              -e SEMGREP_BRANCH=${SEMGREP_BRANCH} \
              -e SEMGREP_REPO_NAME=${SEMGREP_REPO_NAME} \
              -e SEMGREP_COMMIT=${SEMGREP_COMMIT} \
              -e SEMGREP_PR_ID=${SEMGREP_PR_ID} \
              -v ${WORKSPACE}:/src \
              ${semgrepImage} semgrep --config=auto PATH/TO/https://github.com/DorukanUysal/Test
              ${semgrepImage} semgrep ci"

          bat(semgrepCmd) // For Windows
          // sh(semgrepCmd) // For Unix-based systems (Linux, macOS)
        }
      }
    }
  }
}

