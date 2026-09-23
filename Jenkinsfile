node {
    def appDir = '/var/www/nextjs-app/my-app'

    stage('Clean workspace') {
        echo 'Cleaning workspace...'
        deleteDir()
    }

    stage('Clone') {
        echo 'Cloning repository...'
        git branch: 'main', url: 'https://github.com/Dhvanit-raval/CI-CD-Jenkins-AWS.git'
    }

    stage('Deploy') {
        echo 'Deploying application to EC2...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}/

            cd ${appDir}

            # Install dependencies with lower memory footprint
            npm install --no-audit --prefer-offline
            npm run build

            # Stop previous running server on port 3000
            sudo fuser -k 3000/tcp || true

            # Start Next.js app in background
            JENKINS_NODE_COOKIE=dontKillMe nohup npm run start > app.log 2>&1 &
        """
    }
}
