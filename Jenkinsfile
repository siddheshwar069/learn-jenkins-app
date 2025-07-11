pipeline{
    agent any

    stages{
        stage("build"){
            agent{
                docker{
                    image 'node:18-alpine'
                    args '-u root'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    args '-u root'
                    reuseNode true
                }
            }
            steps{
                sh'''
                    echo "testing started..."
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
}