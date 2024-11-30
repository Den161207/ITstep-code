pipeline {
    agent any

    environment {
        APACHE2_SERVICE = 'apache2' // Назва сервісу Apache2
        APACHE2_CONF_DIR = '/etc/apache2' // Директорія з конфігураціями
        APACHE2_CONF_FILE = 'apache2.conf' // Файл конфігурації, який потрібно змінити
    }

    stages {
        stage('Update and Restart Apache2') {
            steps {
                script {
                    // Зупиняємо Apache2 перед змінами
                    echo "Stopping Apache2..."
                    sh "sudo systemctl stop ${env.APACHE2_SERVICE}"

                    // Припустимо, зміни зберігаються в Git-репозиторії
                    echo "Applying configuration changes..."

                    // Клонування репозиторію з конфігураціями
                    sh 'git clone https://your-repository-url/configurations.git'

                    // Копіюємо змінений конфігураційний файл до директорії Apache2
                    sh "sudo cp configurations/${env.APACHE2_CONF_FILE} ${env.APACHE2_CONF_DIR}/${env.APACHE2_CONF_FILE}"

                    // Перевіряємо синтаксис конфігурації
                    sh 'sudo apache2ctl configtest'

                    // Перезапускаємо Apache2 після застосування змін
                    echo "Restarting Apache2..."
                    sh "sudo systemctl restart ${env.APACHE2_SERVICE}"
                }
            }
        }

        stage('Verify Apache2 Status') {
            steps {
                script {
                    // Перевіряємо статус Apache2 після перезапуску
                    echo "Verifying Apache2 status..."
                    sh "sudo systemctl status ${env.APACHE2_SERVICE} --no-pager"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            echo 'Apache2 has been successfully updated and restarted.'
        }
        failure {
            echo 'There was an issue with updating Apache2.'
        }
    }
}
