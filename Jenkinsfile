pipeline {
    agent any

    environment {
        SLACK_CHANNEL = '#jenkins-builds'
        // ID du credential que tu as créé dans Jenkins (Secret Text)
        SLACK_CRED_ID = 'slack-boot'   // <-- change si ton ID est 'slack-webhook'
        GIT_REPO = 'https://github.com/nell852/WebApp.git'
        GIT_BRANCH = 'dev'
    }

    stages {
        stage('Clone') {
            steps {
                echo "🔁 Clonage de ${GIT_REPO} (${GIT_BRANCH})"
                // Si ton repo est public :
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
                // Si ton repo est privé, utilise 'credentialsId: "your-git-cred-id"' dans la ligne git.
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
                // place ici tes commandes de build, ex: sh 'npm install' ou mvn package
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
                echo ":test_tube: Exécution des tests..."
                // place ici tes commandes de test, ex: sh 'pytest' ou npm test
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
                echo ":rocket: Déploiement en cours..."
                // commandes de déploiement
            }
            post {
                success {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":white_check_mark: Stage *Deploy* réussi pour ${env.JOB_NAME} #${env.BUILD_NUMBER}\nLien: ${env.BUILD_URL}")
                }
                failure {
                    slackSend(channel: "${SLACK_CHANNEL}",
                              tokenCredentialId: "${SLACK_CRED_ID}",
                              message: ":x: Stage *Deploy* échoué pour ${env.JOB_NAME} #${env.BUILD_NUMBER}\nLien: ${env.BUILD_URL}")
                }
            }
        }
    }

    post {
        success {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'good',
                      message: ":white_check_mark: *Build réussi !*\n*Projet:* ${env.JOB_NAME}\n*Build:* #${env.BUILD_NUMBER}\n*Durée:* ${currentBuild.durationString}\n:Lien: ${env.BUILD_URL}")
        }
        failure {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'danger',
                      message: ":x: *Build échoué !*\n*Projet:* ${env.JOB_NAME}\n*Build:* #${env.BUILD_NUMBER}\n*Durée:* ${currentBuild.durationString}\n:Lien: ${env.BUILD_URL}")
        }
        unstable {
            slackSend(channel: "${SLACK_CHANNEL}",
                      tokenCredentialId: "${SLACK_CRED_ID}",
                      color: 'warning',
                      message: ":warning: *Build instable !*\n*Projet:* ${env.JOB_NAME}\n*Build:* #${env.BUILD_NUMBER}\n*Durée:* ${currentBuild.durationString}\n:Lien: ${env.BUILD_URL}")
        }
    }
}
