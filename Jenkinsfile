pipeline {
    agent any

    environment {
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
                    sh '''
                    helm upgrade --install $RELEASE_NAME ./appchart/$CHART_PATH --namespace $NAMESPACE --create-namespace
                    '''
                }
            }
        }

        stage('Get Pods') {
            steps {
                script {
                    sh '''
                    kubectl get pods -n $NAMESPACE
                    '''
                }
            }
        }
    }
}
