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
        stage('Check for Changes in Github') {
            steps {
                echo "Checking for changes in Github..."
                script {
                    def workspaceDir = pwd()
                    def gitChanged = false
                    
                    // Git 'status' command to check for changes
                    def statusCmd = "git status --porcelain"
                    def status = bat(script: statusCmd, returnStatus: true)
                    if (status == 0) {
                        echo "No changes in Github repository."
                    } else {
                        echo "Changes found in Github repository. Pulling the latest changes..."
                        def pullCmd = "git pull"
                        bat(script: pullCmd, returnStatus: true) ? gitChanged = true : gitChanged = false
                    }
                    
                    if (gitChanged) {
                        echo "Project updated. Performing the build, test, and deploy stages..."
                    } else {
                        echo "No changes. Skipping the build, test, and deploy stages."
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

        /*stage('Run Application') {
            steps {
                bat "docker exec owasp zap-baseline.py -t http://www.example.com/ -I -j --auto -r testreport.html"
            }
        }
*/
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            dir('php-goof-main') {
                    snykSecurity(
                        snykInstallation: 'Synk',
                        snykTokenId: '4f06e630-a651-4f27-bef1-47994a9dd0d4',
                    // place other parameters here
                )
            }
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
                        returntocorp/semgrep semgrep ci
                    """
                }
            }
        }
    }
}

 
