pipeline {
  agent any
  stages {
    stage('Get Write Access'){
      steps {
             bat "chmod 777 \$(pwd)"
      }
    }
  stage('Setting up OWASP ZAP docker container') {
      steps {
        echo "Starting container --> Start"  
            bat "docker run --rm -v \$(pwd):/zap/wrk/:rw --name owasp -dt owasp/zap2docker-live /bin/bash"
        }
  }
    stage('Run Application') {
      steps {
             bat "docker exec owasp zap-baseline.py -t http://www.example.com/ -I -j --auto -r testreport.html"
            }
        }
    stage('Copy Report to Workspace'){
             steps {
                 script {
                     bat '''
                         docker cp owasp:/zap/wrk/testreport.html ${WORKSPACE}/testreport.html
                     '''
                 }
             }
         }
    stage('Stop and Remove Container') {
      steps {
        echo "Removing container"
            bat '''
                docker stop owasp
                docker rm owasp
               '''
             }
         }
    }
}
