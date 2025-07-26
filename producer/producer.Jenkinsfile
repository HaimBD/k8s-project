pipeline {

    agent any

        options { timeout(time: 1, unit: 'HOURS') }

        stages {
            stage('Build') {
                steps {
                    withCredentials([
                    string(credentialsId: 'docker-user', variable: 'USERNAME'),
                    string(credentialsId: 'docker-pass', variable: 'PASSWORD'),
                    string(credentialsId: 'producer-image', variable: 'PRODUCER_IMAGE'),
                    string(credentialsId: 'Dockerhub-repository', variable: 'DOCKERHUB_REPO')
                    ]
                    ){
                    sh 'echo "test..............."'
                    sh 'docker login --username "${USERNAME}" --password "${PASSWORD}"'
                    sh 'docker build -t "${PRODUCER_IMAGE}":"${BUILD_NUMBER}" producer/.'
                    sh 'docker image tag "${PRODUCER_IMAGE}":"${BUILD_NUMBER}" "${DOCKERHUB_REPO}"/"${PRODUCER_IMAGE}":"${BUILD_NUMBER}"'
                    sh 'docker push "${DOCKERHUB_REPO}"/"${PRODUCER_IMAGE}":"${BUILD_NUMBER}"'

                }
            }
        }
            stage('Trigger Release') {
            steps {
                build job: 'CI-consumer-release',
                    wait: false,
                    parameters: [
                        string(name: 'IMG_BUILD', value: "${BUILD_NUMBER}")
                    ]
            }
        }
                }
        }
