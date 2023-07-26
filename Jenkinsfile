pipeline {
  agent any
environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
}

  stages {

 stage('Stop and Remove Container1') {
      steps {
        echo "Removing container"
            bat '''
                    FOR /f "tokens=*" %%i IN ('docker ps -q') DO docker stop %%i



               '''
             }
         }


stage('Setting up OWASP ZAP docker container') {
      steps {
        echo "Starting container --> Start"  
            bat "docker run --rm -v %cd%:/zap/wrk/:rw --name owasp -dt owasp/zap2docker-stable /bin/bash"
        }
  }
    stage('Run Application') {
      steps {
             bat "docker exec owasp zap-baseline.py -t http://www.example.com/ -I -j --auto -r testreport.html"
            }
        }

stage('Build') {
      steps {
        echo 'Building...'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing...'
        snykSecurity(
          snykInstallation: '<Synk>',
          snykTokenId: '<78b1ec19-3e58-4712-b933-52195c781373>',
          // place other parameters here
        )
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
stage('Semgrep-Scan') {
        steps {
            bat '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -e SEMGREP_REPO_URL=$SEMGREP_REPO_URL \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_COMMIT=$SEMGREP_COMMIT \
            -e SEMGREP_PR_ID=$SEMGREP_PR_ID \
            -v "%WORKSPACE%:%WORKSPACE%" --workdir "%WORKSPACE%" \
            returntocorp/semgrep semgrep ci '''
      }
    }


    }
}
