pipeline {
    agent { label "iti-sys-admin-sohaga" }

    stages {
        stage('build') {
            steps {
                echo 'build'
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME_SOHAG', passwordVariable: 'PASSWORD_SOHAG')]) {
                    sh '''
                        docker login -u ${USERNAME_SOHAG} -p ${PASSWORD_SOHAG}
                        docker build -t kareemelkasaby/bakehouseitisohag:v${BUILD_NUMBER} .
                        docker push kareemelkasaby/bakehouseitisohag:v${BUILD_NUMBER}
                    '''
                }
            }
        }
        stage('deploy') {
            
            steps {
                echo 'deploy'
                withCredentials([file(credentialsId: 'kube', variable: 'KUBECONFIG_SOHAG')]) {
                    sh '''
                        mv Deployment/deploy.yaml Deployment/tmp.yaml
                        cat Deployment/tmp.yaml | envsubst > Deployment/deploy.yaml
                        rm -f Deployment/tmp.yaml
                        kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_SOHAG}
                    '''
                }
            }
        }
    }
}
