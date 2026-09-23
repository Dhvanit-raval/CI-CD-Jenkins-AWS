node {
    def appDir = '/var/www/nextjs-app/my-app'

    stage('Clean workspcae') {
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
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            nohup npm run start > app.log 2>&1 &

        """
    }
}
