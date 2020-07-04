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
        DOCKER_HUB = 'gcr.io/tokyo-receiver-264005/'
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
              withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                          // available as an env variable, but will be masked if you try to print it out any which way
                          // note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
                          sh 'echo $PASSWORD'
                          // also available as a Groovy variable
                          echo USERNAME
                          // or inside double quotes for string interpolation
                          echo "username is $USERNAME"
                        }
               container('kaniko') {
                sh '''
                printenv
                ls
                /kaniko/executor -f Dockerfile --force --destination=${DOCKER_HUB}image:$BUILD_NUMBER
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
