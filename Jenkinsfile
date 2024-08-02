pipeline {
    agent any

    environment {
        // Переменные окружения для Helm и kubectl
        KUBECONFIG = credentials('kubeconfig-credentials-id')
        HELM_VERSION = 'v3.7.1'
        REPO_URL = 'https://github.com/chegevaras/appchart.git'
        CHART_PATH = 'app-chart'
        RELEASE_NAME = 'my-release'
        NAMESPACE = 'neoflex'
    }

    stages {
        stage('Install Helm') {
            steps {
                script {
                    // Устанавливаем Helm
                    sh '''
                    if ! [ -x "$(command -v helm)" ]; then
                      curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
                      chmod 700 get_helm.sh
                      ./get_helm.sh --version $HELM_VERSION
                    fi
                    '''
                }
            }
        }

        stage('Clone Repo') {
            steps {
                script {
                    // Клонируем репозиторий
                    sh '''
                    if [ -d "appchart" ]; then
                      rm -rf appchart
                    fi
                    git clone $REPO_URL
                    '''
                }
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                script {
                    // Устанавливаем Helm чарт из локальной директории
                    sh '''
                    helm upgrade --install $RELEASE_NAME ./appchart/$CHART_PATH --kubeconfig $KUBECONFIG --namespace $NAMESPACE --create-namespace
                    '''
                }
            }
        }

        stage('Get Pods') {
            steps {
                script {
                    // Получаем список подов в namespace
                    sh '''
                    kubectl get pods -n $NAMESPACE --kubeconfig $KUBECONFIG
                    '''
                }
            }
        }
    }
}
