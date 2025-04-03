pipeline {
    agent any
    stages {
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -al
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                    echo "Hello Max you growing"
                    whoami
                '''
                stash name: 'build_artifacts', includes: 'build/**'
            }
        }

        stage("Test") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                unstash 'build_artifacts'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage("Deploy") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                unstash 'build_artifacts'
                sh '''
                    npm install -g netlify-cli
                    netlify --version
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
