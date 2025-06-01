pipeline {
    agent any

    environment {
        CONTAINER_NAME = "mysql-container3"
        MYSQL_ROOT_PASSWORD = "root"
        MYSQL_DATABASE = "testdb"
        MYSQL_PORT = "3308"
        GIT_REPO = "https://github.com/Pgdev-ops/sql-deployment.git"
        SQL_FILE_PATH = "deploy.sql"   // Assuming deploy.sql is in repo root
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: "${GIT_REPO}"
            }
        }

        stage('Clean up existing container') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Start MySQL container') {
            steps {
                sh """
                docker run --name ${CONTAINER_NAME} \
                -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} \
                -e MYSQL_DATABASE=${MYSQL_DATABASE} \
                -p ${MYSQL_PORT}:3306 -d mysql:5.7
                """
            }
        }

        stage('Copy SQL script into container') {
            steps {
                sh "docker cp ${SQL_FILE_PATH} ${CONTAINER_NAME}:/deploy.sql"
            }
        }

        stage('Execute SQL script') {
            steps {
                sh "docker exec -i ${CONTAINER_NAME} mysql -u root -p${MYSQL_ROOT_PASSWORD} ${MYSQL_DATABASE} < /deploy.sql"
            }
        }
    }

    post {
        always {
            echo "Stopping and removing MySQL container"
            sh "docker rm -f ${CONTAINER_NAME} || true"
        }
    }
}
