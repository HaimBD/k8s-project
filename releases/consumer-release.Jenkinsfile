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
                    git config --global user.email "haimbendavid1995@gmail.com"
                    git config --global user.name "${USERNAME}"
                    git config --global --add safe.directory /home/ubuntu/jenkins/workspace/CI-consumer-release
                    git fetch -p https://${USERNAME}:${PASSWORD}@github.com/${USERNAME}/k8s-project.git
                    sed -i "s|tag: .*|tag: ${IMG_BUILD}|g" helms/consumer-helm/consumer-helm/values.yaml
                    git add helms/consumer-helm/consumer-helm/values.yaml
                    git commit --allow-empty -m $IMG_BUILD
                    git push https://${USERNAME}:${PASSWORD}@github.com/${USERNAME}/k8s-project.git HEAD:main --force
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
            sh 'echo Adding this........'
            }
        }
}