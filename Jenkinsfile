pipeline {
    agent any

    environment {
        // Переменные окружения для Helm и kubectl
        HELM_VERSION = 'v3.7.1'
        CHART_REPO = 'https://raw.githubusercontent.com/chegevaras/appchart/main'
        CHART_NAME = 'appchart'
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

        stage('Add Helm Repo') {
            steps {
                script {
                    // Добавляем Helm репозиторий
                    sh '''
                    helm repo add myrepo $CHART_REPO
                    helm repo update
                    '''
                }
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                script {
                    // Устанавливаем Helm чарт
                    sh '''
                    helm upgrade --install $RELEASE_NAME myrepo/$CHART_NAME --namespace $NAMESPACE --create-namespace
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
