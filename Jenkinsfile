pipeline {
  agent any
  stages {
  
    stage('Stop and Remove Container') {
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
