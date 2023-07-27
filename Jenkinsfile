pipeline {
    agent any
    environment {
        // The following variable is required for a Semgrep Cloud Platform-connected scan:
        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_REPO_URL = 'https://github.com/DorukanUysal/Test.git'
        SEMGREP_BRANCH = 'main'
        SEMGREP_REPO_NAME = 'Test'
    }

    stages {
        stage('Check for Repo Updates') {
            steps {
                echo "Checking for repository updates..."
                script {
                    // Git kullanarak mevcut dalı ve son güncelleme tarihini al
                    def gitBranch = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    def lastCommitDate = sh(script: 'git log -1 --format=%cd', returnStdout: true).trim()

                    // Git pull komutunu çalıştır ve değişiklikleri kontrol et
                    def pullOutput = sh(script: 'git pull', returnStatus: true)

                    if (pullOutput == 0) {
                        echo "Repository has been updated. Pulling the latest changes..."
                        // OWASP ZAP ve Semgrep taramalarını gerçekleştir
                        runOWASPZAPScan()
                        runSemgrepScan()
                        // Snyk taramasını yalnızca "php-goof-main" dizininde gerçekleştir
                        dir('php-goof-main') {
                            runSnykScan()
                        }
                    } else {
                        echo "Repository is up-to-date. No changes detected."
                    }
                }
            }
        }

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
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }

        stage('Stop and Remove Container2') {
            steps {
                echo "Removing container"
                bat '''
                    FOR /f "tokens=*" %%i IN ('docker ps -q') DO docker stop %%i
                    docker stop jenkins_container
                    docker rm jenkins_container
                '''
            }
        }

        stage('Semgrep-Scan') {
            steps {
                script {
                    // Çalışma alanını Groovy tarafından alın
                    def absWorkspace = pwd()

                    // Batch komutunda Jenkins çalışma alanını mutlak yola çevirin
                    def absWorkspacePath = absWorkspace.replace('\\', '\\\\') // Çift ters eğik çizgi eklenmeli
                    bat """
                        docker pull returntocorp/semgrep
                        docker run -d -p 8081:8080 -v jenkins_data:/var/jenkins_home --name jenkins_container jenkins/jenkins

                        set SEMGREP_APP_TOKEN="${SEMGREP_APP_TOKEN}" ^
                        set SEMGREP_REPO_URL=${SEMGREP_REPO_URL} ^
                        set SEMGREP_BRANCH=${SEMGREP_BRANCH} ^
                        set SEMGREP_REPO_NAME=${SEMGREP_REPO_NAME} ^
                        -v "${absWorkspacePath}:${absWorkspacePath}" --workdir "${absWorkspacePath}" ^




 
