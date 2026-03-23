pipeline {
    agent any
    stages {
        
        stage('Clean Source Repo') {
            steps {
                bat '''
                if exist Jenkins_pipeline (
                    rmdir /s /q Jenkins_pipeline
                )
                '''
            }
        }

        stage('Clone Source Repo') {
            steps {
                bat 'git clone https://github.com/kapilrahtor/Jenkins_pipeline.git'
            }
        }

        stage('Copy Files to My Repo') {
            steps {
                bat '''
                robocopy Jenkins_pipeline . /E /XD .git
                if %ERRORLEVEL% LEQ 7 exit /B 0
                '''
            }
        }

        stage('Commit & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'USER', passwordVariable: 'TOKEN')]) {
                    bat '''
                    git config user.email "pranavsharma2904@gmail.com"
                    git config user.name "PranavSharma112"
                    git add .
                    git commit -m "Auto copied files from Kapil repo" || exit 0
                    git push https://%USER%:%TOKEN%@github.com/PranavSharma112/Jenkins.git HEAD:main
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                bat 'Build.bat'
            }
        }

        stage('Test') {
            steps {
                bat 'Test.bat'
            }
        }

        stage('Deploy') {
            steps {
                bat 'Deploy.bat'
            }
        }
    }
}
