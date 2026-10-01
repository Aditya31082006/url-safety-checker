pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/jenkins-demo'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building website...'

                sh '''
                    rm -rf dist
                    mkdir -p dist

                    cp index.html dist/
                    cp style.css dist/

                    echo "Build completed successfully."
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website...'

                sh '''
                    test -f dist/index.html
                    test -f dist/style.css

                    grep -qi "<html" dist/index.html
                    grep -qi "<title>" dist/index.html
                    grep -qi "</html" dist/index.html

                    echo "All tests passed."
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website...'

                sh '''
                    rsync -av --delete dist/ "${DEPLOY_DIR}/"
                    chmod -R 755 "${DEPLOY_DIR}"

                    echo "Deployment completed successfully."
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying deployment...'

                sh '''
                    sleep 2
                    curl --fail --silent http://localhost:8081 > /dev/null

                    echo "Deployment verified successfully."
                '''
            }
        }
    }
}
