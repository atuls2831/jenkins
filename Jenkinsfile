pipeline {
    agent {
        label '!windows'
    }

    environment {
        DISABLE_AUTH = 'false'
        DB_ENGINE    = 'sqlite'
    }

    stages {
        stage('Build') {
            steps {
                echo "Database engine is ${DB_ENGINE}"
                echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                sh 'printenv'
            }
        }
         stage('Test') {
            steps {
                echo "My Database engine is ${DB_ENGINE}"
                echo "My DISABLE_AUTH is ${DISABLE_AUTH}"
                sh 'printenv'
            }
        }
    }
}
