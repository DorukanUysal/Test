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
stage('Clone Repository') {
            steps {
                // Git repo klonlamak için gerekli adımlar
                bat 'git clone https://github.com/DorukanUysal/Test.git'
            }
        }

        stage('Run Semgrep in Docker') {
            steps {
                // Semgrep'i Docker konteynerinde çalıştırmak için gerekli adımlar
                script {
                    // Docker konteyneri çalışma dizini olarak Jenkins çalışma alanını kullanıyoruz
                    def workspaceDir = pwd()

                    // Semgrep komutunu Docker konteyneri üzerinde çalıştırmak
                    // -e ile çevresel değişkenler geçirebiliriz
                    // -v ile Jenkins çalışma alanını Docker konteyneri içinde bağlayabiliriz
                    // İşinize göre SEMGREP_REPO_NAME, SEMGREP_BRANCH, SEMGREP_COMMIT, SEMGREP_PR_ID gibi değişkenleri ayarlamanız gerekebilir
                    // Bu değişkenler, Semgrep'in çalışacağı projenin bilgilerini içerir
                    // Docker görüntüsü returntocorp/semgrep'i kullanıyoruz
                    bat "docker run --rm -e SEMGREP_REPO_NAME=<repo_name> -e SEMGREP_BRANCH=<branch_name> -e SEMGREP_COMMIT=<commit_hash> -v \"%workspaceDir%\":\"%workspaceDir%\" --workdir \"%workspaceDir%\" returntocorp/semgrep semgrep ci"
                }
            }
        }
    
  
    }
}
