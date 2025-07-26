pipeline {

    agent any
        parameters {
                     string(name: 'IMG_BUILD', defaultValue: '', description: '')
                     }

        options { timeout(time: 1, unit: 'HOURS') }

        stages {
            stage('Update YAML') {
                steps {
                    withCredentials([
                    string(credentialsId: 'github-username', variable: 'USERNAME'),
                    string(credentialsId: 'github-password', variable: 'PASSWORD')
                    ])

                   {

                    sh '''


                    if [[ ${IMG_URL} =~ "poly" ]]; then
                        echo "poly"
                        YAML_FILE=k8s/dev/polybot-dev.yaml
                    elif [[ ${IMG_URL} =~ "yolo5" ]]; then
                        echo "yolo5"
                        YAML_FILE=k8s/dev/yolo-dev.yaml


                    else
                        echo "not met"
                        exit 7
                    fi

                    git config --global user.email "haimbendavid1995@gmail.com"
                    git config --global user.name "HaimBD"
                    git config --global --add safe.directory /home/ubuntu/jenkins/workspace/releases/releases-dev
                    git fetch -p https://${USERNAME}:${PASSWORD}@github.com/HaimBD/cicd-project.git
                    git checkout -f releases
                    git merge -X theirs origin/main
                    sed -i "s|image: .*|image: ${IMG_URL}|g" ${YAML_FILE}
                    cat ${YAML_FILE}
                    git add ${YAML_FILE}
                    git commit --allow-empty -m $IMG_URL
                    git push https://${USERNAME}:${PASSWORD}@github.com/HaimBD/cicd-project.git releases --force
                    '''
                    }
                }

            }
        }

        post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE']])
            sh 'docker system prune -a -f --filter "until=24h"'
            sh 'echo Adding this............'
            }
        }
}