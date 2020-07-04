pipeline {
            agent {
                        kubernetes {
                        label 'agent-docker-create-setup'
                        yaml '''
                                apiVersion: v1
                                kind: Pod
                                spec:
                                  containers:
                                  - name: "jnlp"
                                    resources:
                                      requests:
                                        cpu: 200m
                                        memory: 400Mi
                                      limits:
                                        cpu: 400m
                                        memory: 800Mi    
                                  - name: kaniko
                                    image: gcr.io/kaniko-project/executor:debug 
                                    resources:
                                      requests:
                                        cpu: 200m
                                        memory: 400Mi
                                      limits:
                                        cpu: 400m
                                        memory: 800Mi       
                                    command:
                                    - /busybox/sh
                                    tty: true
                                '''
                                }
            }

    environment {
        DISABLE_AUTH = 'True'
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
        stage('My sample stage') {
            steps {
               container('kaniko') {
                sh '''
                printenv
                ls
                /kaniko/executor -f Dockerfile --no-push
                '''
            }
                
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
