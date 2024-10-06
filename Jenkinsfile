pipeline {
    agent any
    stages {
        stage("Build"){
            agent{
                docker{
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
            }
        }
       stage ("Test") (
            sh 'test -f build/index.html'
       )
    }
}