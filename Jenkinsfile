pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '1a5089dd-2ee0-4e99-998c-a4392812992b'
        NETLIFY_AUTH_TOKEN =credentials('netlify-token')

    }

    stages {
        stage('build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
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
                    reuseNode true
                }

            }
            steps{
                sh '''
                test -f build/index.html
                npm test
                '''
            }

        }

        stage('deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                 npm install netlify-cli 
                 node_modules/.bin/netlify --version
                 echo "deploy to production , site id : $NETLIFY_SITE_ID"
                 node_modules/.bin/netlify status
                 node_modules/.bin/netlify deploy --dir=build --prod


                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
