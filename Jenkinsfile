pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Pgdev-ops/sql-deployment.git'
            }
        }

        stage('Deploy SQL to MySQL') {
            steps {
                script {
                    def dbUser = 'root'
                    def dbPass = 'root'
                    def dbName = 'testdb'

                    sh "docker cp ${WORKSPACE}/deploy.sql mysql-container:/deploy.sql"

                    sh """
                        docker exec mysql-container bash -c \\
                        "mysql -u${dbUser} -p${dbPass} ${dbName} < /deploy.sql"
                    """
                }
            }
        }
    }
}
