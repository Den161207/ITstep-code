pipeline {
    agent any
    
    environment {
        // Задаємо змінні середовища для серверів
        APACHE2_SERVICE = 'apache2'
        NGINX_SERVICE = 'nginx'
        TOMCAT_SERVICE = 'tomcat'
    }
    
    stages {
        stage('Stop Services') {
            steps {
                script {
                    // Зупинка сервісів
                    sh "sudo systemctl stop ${env.APACHE2_SERVICE}"
                    sh "sudo systemctl stop ${env.NGINX_SERVICE}"
                    sh "sudo systemctl stop ${env.TOMCAT_SERVICE}"
                }
            }
        }
        
        stage('Apply Configuration Changes') {
            steps {
                script {
                    // Припустимо, що зміни конфігурацій зберігаються у Git
                    // Клонування репозиторію з конфігураціями
                    sh 'git clone https://ITstep-code-url/configurations.git'
                    
                    // Копіюємо конфігурації до відповідних директорій
                    sh 'sudo cp configurations/apache2.conf /etc/apache2/apache2.conf'
                    sh 'sudo cp configurations/nginx.conf /etc/nginx/nginx.conf'
                    sh 'sudo cp configurations/server.xml /opt/tomcat/conf/server.xml'
                }
            }
        }
        
        stage('Restart Services') {
            steps {
                script {
                    sh "sudo systemctl start ${env.APACHE2_SERVICE}"
                    sh "sudo systemctl start ${env.NGINX_SERVICE}"
                    sh "sudo systemctl start ${env.TOMCAT_SERVICE}"
                }
            }
        }
        
        stage('Verify Services') {
            steps {
                script {
                    sh "sudo systemctl status ${env.APACHE2_SERVICE} --no-pager"
                    sh "sudo systemctl status ${env.NGINX_SERVICE} --no-pager"
                    sh "sudo systemctl status ${env.TOMCAT_SERVICE} --no-pager"
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution finished!'
        }
        success {
            echo 'Services have been successfully updated and restarted.'
        }
        failure {
            echo 'There was an issue with the services update.'
        }
    }
}

