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
stage('Semgrep-Scan') {
        steps {
           powershell '''
        docker pull returntocorp/semgrep
        docker run `
            -e SEMGREP_APP_TOKEN=$env:SEMGREP_APP_TOKEN `
            -e SEMGREP_REPO_URL=$env:SEMGREP_REPO_URL `
            -e SEMGREP_BRANCH=$env:SEMGREP_BRANCH `
            -e SEMGREP_REPO_NAME=$env:SEMGREP_REPO_NAME `
            -e SEMGREP_BRANCH=$env:SEMGREP_BRANCH `
            -e SEMGREP_COMMIT=$env:SEMGREP_COMMIT `
            -e SEMGREP_PR_ID=$env:SEMGREP_PR_ID `
            -v "${env:WORKSPACE}:${env:WORKSPACE}" --workdir "${env:WORKSPACE}" `
            returntocorp/semgrep semgrep ci
        '''
      }
    }


    }
}
