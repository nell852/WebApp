pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#jenkins-builds'
        SLACK_CRED_ID = 'slack-boot'
        GIT_REPO = 'git@github.com:nell852/WebApp.git'
        GIT_CRED_ID = 'jenkins-github-ssh'

        DOCKERHUB_CRED_ID = 'dockerhub-cred'
        DOCKER_IMAGE = 'nellblaise/webapp'
        APP_PORT = '8084'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Clone') {
            steps {
                echo "🔁 Clonage du dépôt ${GIT_REPO}"
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: "${GIT_REPO}", credentialsId: "${GIT_CRED_ID}"]]
                ])
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":white_check_mark: Étape Clone réussie ✅")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":x: Étape Clone échouée ❌")
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo ":construction: Construction de l'image Docker..."
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":white_check_mark: Étape Build Docker réussie ✅")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":x: Étape Build Docker échouée ❌")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo ":whale: Envoi de l'image vers Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CRED_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":white_check_mark: Étape Push Docker réussie ✅")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":x: Étape Push Docker échouée ❌")
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo ":rocket: Déploiement de l'image Docker sur le port ${APP_PORT}"
                script {
                    sh """
                        docker stop webapp || true
                        docker rm webapp || true
                        docker pull ${DOCKER_IMAGE}:latest
                        docker run -d -p ${APP_PORT}:80 --name webapp ${DOCKER_IMAGE}:latest
                    """
                }
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":white_check_mark: Étape Deploy réussie ✅")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":x: Étape Deploy échouée ❌")
                }
            }
        }
    }

    post {
        success {
            slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":tada: Pipeline complet réussi ! Toutes les étapes ✅")
        }
        failure {
            slackSend(channel: "${SLACK_CHANNEL}", tokenCredentialId: "${SLACK_CRED_ID}", message: ":x: Pipeline échoué ! Une ou plusieurs étapes ❌")
        }
    }
}
