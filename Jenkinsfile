pipeline {
    agent any
    environment { 
    def COV_DIR = <Path to Coverity binary>
    }
    stages{
        stage('Build'){
            steps {
                sh 'echo $COV_DIR'
                sh './configure'
                sh 'make'
            }
            post {
                success {
                    echo 'Build successfully!'
                }
            }
        }
        stage('Clean'){
            steps {
                sh 'make clean'
            }
        }
        stage('Coverity build'){
            steps {
                sh '$COV_DIR/cov-build --dir idir make'
            }
            post {
                success {
                    echo 'Coverity build successfully!'
                }
                failure {
                    echo 'Coverity build failed!'
                }
            }
        }
        stage('Coverity analyze'){
            steps {
                sh '$COV_DIR/cov-analyze --dir idir --all'
            }
            post {
                success {
                    echo 'Coverity analyze successfully!'
                }
                failure {
                    echo 'Coverity analyze failed!'
                }
            }
        }
        stage('Coverity commit'){
            steps {
                sh '$COV_DIR/cov-commit-defects --dir idir --host hostname --port <port> --auth-key-file $COV_DIR/auth-key-admin --stream stream'
            }
            post {
                success {
                    echo 'Coverity commit successfully!'
                }
                failure {
                    echo 'Coverity commit failed!'
                }
            }
        }
    }
}