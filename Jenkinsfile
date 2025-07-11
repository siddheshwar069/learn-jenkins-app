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
        stage('e2etests'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.54.0-noble'
                    reuseNode true
                }
            }
            steps{
                sh'''
                    npm install serve
                    serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post{
        always{
            junit "test-results/junit.xml"
        }
    }
}