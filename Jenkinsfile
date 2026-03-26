pipeline {
    agent any

    stages {
        stage('🔍 Clone Code') {
            steps {
                echo '======== CLONING FROM GIT ========'
                git branch: 'main', 
                    url: 'https://github.com/YOUR-USERNAME/my-jenkins-project.git'
                echo '✅ Code cloned successfully!'
            }
        }

        stage('📦 Build') {
            steps {
                echo '======== BUILDING PROJECT ========'
                sh 'ls -la'
                echo '✅ Build successful!'
            }
        }

        stage('🚀 Deploy to Nginx') {
            steps {
                echo '======== DEPLOYING TO NGINX ========'
                
                script {
                    // Stop and remove existing container (if any)
                    sh '''
                        docker stop nginx-web 2>/dev/null || true
                        docker rm nginx-web 2>/dev/null || true
                    '''
                    
                    // Create directory for HTML files
                    sh '''
                        mkdir -p /tmp/nginx-html
                        cp index.html /tmp/nginx-html/
                        cp styles.css /tmp/nginx-html/
                    '''
                    
                    // Run Nginx container
                    sh '''
                        docker run -d \
                          --name nginx-web \
                          -p 80:80 \
                          -v /tmp/nginx-html:/usr/share/nginx/html:ro \
                          nginx:latest
                    '''
                }
                
                echo '✅ Nginx deployed successfully!'
            }
        }

        stage('✅ Verify Deployment') {
            steps {
                echo '======== VERIFYING DEPLOYMENT ========'
                sh 'sleep 3 && curl -s http://localhost | head -20'
                echo '✅ Verification complete!'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
