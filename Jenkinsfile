pipeline {
  agent any
  stages {


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
    
    stage('Stop and Remove Container2') {
      steps {
        echo "Removing container"
            bat '''
                docker stop owasp/zap2docker-stable
                docker rm owasp/zap2docker-stable
               '''
             }
         }
    }
}
