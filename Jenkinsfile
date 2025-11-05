pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#jenkins-builds'
        SLACK_CRED_ID = 'slack-boot'            // Ton credential Slack dans Jenkins
        GIT_REPO = 'https://github.com/nell852/WebApp.git'
    }

    triggers {
        githubPush() // Déclenchement automatique via webhook
    }

    stages {
        stage('Clone') {
            steps {
                echo "🔁 Clonage de la branche déclenchante depuis ${GIT_REPO}"
                // Checkout automatique de la branche qui a reçu le push
                checkout scm
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
        unstable {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'warning',
                      message: """:warning: *Build global instable !*
*Projet:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Durée:* ${currentBuild.durationString}
:link: ${env.BUILD_URL}""")
        }
    }
}
