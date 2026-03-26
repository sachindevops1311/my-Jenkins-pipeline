pipeline {
    agent any

    stages {
        stage('📝 Create Files') {
            steps {
                echo '======== CREATING HTML FILES ========'
                
                // Create index.html
                sh '''cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jenkins Pipeline Demo</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>🚀 Jenkins CI/CD Pipeline</h1>
        <p>This page is deployed using Jenkins and Nginx!</p>
        <div class="info">
            <h2>Pipeline Stages:</h2>
            <ul>
                <li>✅ Create Files</li>
                <li>✅ Build</li>
                <li>✅ Deploy on Nginx</li>
                <li>✅ Display HTML</li>
            </ul>
        </div>
        <p><strong>Deployed at:</strong> <span id="timestamp"></span></p>
    </div>
    <script>
        document.getElementById('timestamp').textContent = new Date().toLocaleString();
    </script>
</body>
</html>
EOF'''
                
                // Create styles.css
                sh '''cat > styles.css << 'EOF'
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    background: white;
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
    text-align: center;
    max-width: 600px;
}

h1 {
    color: #333;
    margin-bottom: 20px;
    font-size: 2.5em;
}

p {
    color: #666;
    font-size: 1.1em;
    margin-bottom: 30px;
}

.info {
    background: #f5f5f5;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    text-align: left;
}

.info h2 {
    color: #667eea;
    margin-bottom: 15px;
}

.info ul {
    list-style: none;
    font-size: 1.05em;
    color: #555;
}

.info li {
    margin: 10px 0;
    padding-left: 20px;
}

#timestamp {
    color: #667eea;
    font-weight: bold;
}
EOF'''
                
                echo '✅ Files created!'
                sh 'ls -la *.html *.css'
            }
        }

        stage('📦 Build') {
            steps {
                echo '======== BUILDING PROJECT ========'
                sh 'echo "Build process completed"'
                echo '✅ Build successful!'
            }
        }

        stage('🚀 Deploy to Nginx') {
            steps {
                echo '======== DEPLOYING TO NGINX ========'
                
                script {
                    sh '''
                        # Stop and remove existing container
                        docker stop nginx-web 2>/dev/null || true
                        docker rm nginx-web 2>/dev/null || true
                        
                        # Create directory and copy files
                        mkdir -p /tmp/nginx-html
                        cp index.html /tmp/nginx-html/
                        cp styles.css /tmp/nginx-html/
                        
                        # Run Nginx container
                        docker run -d \
                          --name nginx-web \
                          -p 80:80 \
                          -v /tmp/nginx-html:/usr/share/nginx/html:ro \
                          nginx:latest
                        
                        echo "Nginx container started!"
                    '''
                }
                
                echo '✅ Nginx deployed successfully!'
            }
        }

        stage('✅ Verify Deployment') {
            steps {
                echo '======== VERIFYING DEPLOYMENT ========'
                sh '''
                    sleep 3
                    echo "Checking Nginx status..."
                    docker ps | grep nginx-web
                    echo "Testing connection..."
                    curl -s http://localhost | head -20
                '''
                echo '✅ Verification complete!'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline executed successfully!'
            echo '📍 Access your page at: http://13.201.167.79'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
