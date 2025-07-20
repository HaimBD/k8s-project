pipeline {

    agent {
        any

        options { timeout(time: 1, unit: 'HOURS') }

        stages {
            stage('Build') {
                steps {
                    withCredentials([
                    string(credentialsId: 'docker-user', variable: 'USERNAME'),
                    string(credentialsId: 'docker-pass', variable: 'PASSWORD')
                    ]
                    ){
                    sh 'echo "test........"'
                    sh 'docker login --username "${USERNAME}" --password "${PASSWORD}"'
                    sh 'docker build -t rabbitmq-consumer:"${BUILD_NUMBER}" k8s/dev/polybot/.'
                    sh 'docker image tag poly-k8s-dev:"${BUILD_NUMBER}" haim77420/poly-k8s-dev:"${BUILD_NUMBER}"'
                    sh 'docker push haim77420/poly-k8s-dev:"${BUILD_NUMBER}"'

                }
            }
        }
            stage('Trigger Release')  {
                    steps {
                        build job: 'releases/releases-dev', wait: false, parameters: [
                            string(name: 'IMG_URL', value: "haim77420/poly-k8s-dev:${BUILD_NUMBER}")]
                    }
                }
        }
}