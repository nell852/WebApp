pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#jenkins-builds'
        SLACK_CRED_ID = 'slack-boot'
        GIT_REPO = 'https://github.com/nell852/WebApp.git'
        MAIN_BRANCH = 'main'
    }

    triggers {
        githubPush() // déclenchement auto via webhook (ngrok requis si localhost)
    }

    stages {
        stage('Clone') {
            steps {
                echo "🔁 Clonage du dépôt (${MAIN_BRANCH}) depuis ${GIT_REPO}"
                git branch: "${MAIN_BRANCH}", url: "${GIT_REPO}"
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":white_check_mark: Stage *Clone* réussi pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":x: Stage *Clone* échoué pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Build') {
            steps {
                echo ":construction: Construction du projet..."
                sh 'echo "Simulation du build..." && sleep 2'
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":white_check_mark: Stage *Build* réussi pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":x: Stage *Build* échoué pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Tests') {
            steps {
                echo ":test_tube: Lancement des tests..."
                sh 'echo "Simulation des tests..." && sleep 2'
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":white_check_mark: Stage *Tests* réussi pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":x: Stage *Tests* échoué pour ${env.JOB_NAME} #${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Deploy') {
            steps {
                echo ":rocket: Déploiement du projet..."
                sh 'echo "Simulation du déploiement..." && sleep 2'
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":white_check_mark: Stage *Deploy* réussi pour ${env.JOB_NAME} #${env.BUILD_NUMBER}\n:link: ${env.BUILD_URL}")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":x: Stage *Deploy* échoué pour ${env.JOB_NAME} #${env.BUILD_NUMBER}\n:link: ${env.BUILD_URL}")
                }
            }
        }
    }

    post {
        success {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'good',
                      message: """:white_check_mark: *Build global réussi !*
*Projet:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Durée:* ${currentBuild.durationString}
:link: ${env.BUILD_URL}""")
        }
        failure {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'danger',
                      message: """:x: *Build global échoué !*
*Projet:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Durée:* ${currentBuild.durationString}
:link: ${env.BUILD_URL}""")
        }
    }
}
