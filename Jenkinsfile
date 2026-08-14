pipeline {
    agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
    environment {
        NETLIFY_SITE_ID = 'e7a5548b-d185-481a-ad95-06993f6d4c8b'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                   ls -al
                   node --version
                   npm --version
                   npm ci
                   npm run build
                   ls -al 
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Test stage'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E') {
            steps {
                sh '''
                    npm install -g serve
                    serve -s build & sleep 10
                    npx playwright test
                    npx playwright test --reporter=html
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    npm install -g netlify-cli@20.1.1
                    netlify --version
                    echo "프로젝트 배포중... 사이트 아이디 : $NETLIFY_SITE_ID"
                '''
            }
        }

    }
    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}
