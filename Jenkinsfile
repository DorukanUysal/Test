pipeline {
  agent any
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
            bat '''docker pull returntocorp/semgrep && \
            docker run \
            docker run -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME -e SEMGREP_BRANCH=$SEMGREP_BRANCH -e SEMGREP_COMMIT=$SEMGREP_COMMIT -e SEMGREP_PR_ID=$SEMGREP_PR_ID -v "%cd%:%cd%" --workdir %cd% returntocorp/semgrep semgrep ci
'''
      }
    }
    
  
    }
}
