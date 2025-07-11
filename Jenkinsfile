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
        
        stage('e2etests'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.54.0-noble'
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps{
                sh'''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 20
                    npx playwright test
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
    post{
        always{
            junit "test-results/junit.xml"
        }
    }
}