pipeline {
  agent any
  stages {
  
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
