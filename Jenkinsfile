pipeline {
    agent any
    environment {
        DEPLOY_SERVER = "142.93.222.67"   // App server IP
        DEPLOY_USER = "root"              // SSH user
        DEPLOY_PATH = "/var/www/java-app" // Deployment directory
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying branch: ${env.BRANCH_NAME}"
                    echo "Repository URL: ${env.GIT_URL}"
                }
                sshagent(['your-ssh-credential-id']) { // Replace with your Jenkins SSH Credential ID
                    sh """
                    ssh ${DEPLOY_USER}@${DEPLOY_SERVER} 'mkdir -p ${DEPLOY_PATH}'
                    scp target/*.jar ${DEPLOY_USER}@${DEPLOY_SERVER}:${DEPLOY_PATH}/app.jar
                    ssh ${DEPLOY_USER}@${DEPLOY_SERVER} 'nohup java -jar ${DEPLOY_PATH}/app.jar > ${DEPLOY_PATH}/app.log 2>&1 &'
                    """
                }
            }
        }
    }
    post {
        always {
            echo "Pipeline completed for branch: ${env.BRANCH_NAME}"
        }
    }
}
