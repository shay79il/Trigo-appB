properties([githubProjectProperty(displayName: '', projectUrlStr: 'https://github.com/shay79il/Trigo-appB/'), parameters([[$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: false, name: 'ENV_NAME', randomName: 'choice-parameter-325037495641708', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return [\'DEV\']'], script: [classpath: [], sandbox: false, script: 'return [\'DEV\',\'PRODUCTION\']']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: false, name: 'REGION', randomName: 'choice-parameter-325037497993625', referencedParameters: 'ENV_NAME', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''def choices
switch(ENV_NAME){
    case \'PRODUCTION\':
        choices = [\'eu-central-1\', \'eu-central-2\', \'eu-central-3\']
        break
    case \'DEV\':
        choices = [\'eu-west-1\', \'eu-west-2\']
        break
    default:
        choices = [\'eu-west-1\']
        break
}
return choices''']]], string(defaultValue: '1', name: 'REPLICAS')]), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([githubPush()])])
pipeline {
    agent any

    stages {
        stage ('(1) Git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/shay79il/Trigo-appB'
            }
        }
        stage ('(2) Build image') {
            steps {
                sh 'docker build -t shay79il/app-b .'
            }
        }
    }

    post {
        success {
            echo "${env.BUILD_URL}"
            echo "======= SUCCESS =======\n======= SUCCESS =======\n======= SUCCESS =======\n"

            // stage ('(1) Push image to dockerHub')
            sh 'docker push shay79il/app-b'

            // sudo cp .kube/config ~jenkins/.kube/config
            // sudo chown jenkins: ~jenkins/.kube/config
            // stage ('(2) Add Helm Chart repo')
            sh 'helm repo add myhelmrepo https://shay79il.github.io/helm-chart/'

            // stage ('(3) Update Helm Chart repo')
            sh 'helm repo update myhelmrepo'

            // stage ('(4) Deploy Helm Chart App_A')
            sh 'helm upgrade --install app-b myhelmrepo/trigo-app-b  \
                --set app.region=${REGION} \
                --set app.env=${ENV_NAME} \
                --set app.replicas=${REPLICAS}'
        }
        failure {
            echo "${env.BUILD_URL}"
            echo "======= FAIL !!! =======\n======= FAIL !!! =======\n======= FAIL !!! =======\n"
            echo "======= Build docker image \nshay79il/app-b FAILED!!! =======\n"
        }
    }
}