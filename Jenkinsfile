
pipeline {
  agent any
def semgrepImage = "docker.io/returntocorp/semgrep:latest"

    stages {
 stage('Run Sempgrep') {
            steps {
                script {
                    docker.image(semgrepImage).inside {
                        sh 'semgrep --config=auto'
                    }
                }
            }
        }
      stage('Semgrep-Scan') {
        steps {
            bat '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_REPO_URL=$SEMGREP_REPO_URL \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_COMMIT=$SEMGREP_COMMIT \
            -e SEMGREP_PR_ID=$SEMGREP_PR_ID \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            ${semgrepImage} semgrep --config=auto
              ${semgrepImage} semgrep ci"

            returntocorp/semgrep semgrep ci '''

      }
    }
  }
}
