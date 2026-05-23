pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Окружение для деплоя')
    }

    stages {
        stage('Print Info') {
            steps {
                echo "Deploying to ${params.ENV}"
                sh 'echo "Branch: $(git rev-parse --abbrev-ref HEAD)"'
                sh 'echo "Hash: $(git rev-parse HEAD)"'
            }
        }

        stage('Deploy via SSH') {
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: "node1",
                            transfers: [
                                sshTransfer(
                                    sourceFiles: "main.py,requirements.txt",
                                    remoteDirectory: "app-${params.ENV}"
                                )
                            ],
                            verbose: true
                        )
                    ]
                )
            }
        }

        stage('Clean workspace') {
            steps {
                deleteDir()
            }
        }
    }

    post {
        success {
            echo 'Deploy completed successfully'
        }
        failure {
            echo 'Deploy failed'
        }
    }
}
