/* Jenkinsfile (Declarative Pipeline) */
pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'echo "Fail!"; exit 1'
            }
        }
    }
    /* Kind of like 'finally' */
    post {
        always {
            echo 'This will alwasy run'
            /* This can be used for stuff like assembling the results of all tests run during the pipeline execution lifetime.
            See: https://www.jenkins.io/doc/pipeline/tour/tests-and-artifacts/ */
        }
        success {
            echo 'This will only run if the stages executed successfully.'

            /* Notification example:
            slackSend channel: '#ops-room',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully." */
        }
        failure {
            echo 'This will only run when at least one of the stages failed.'

            /* Notification example:
            mail to: 'team@example.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"

            OR

            hipchatSend message: "Attention @here ${env.JOB_NAME} #${env.BUILD_NUMBER} has failed.",
                color: 'RED' */
        }
        unstable {
            echo 'This will only run if the run was marked as unstable.'

            /* A pipeline that has failing tests will be marked UNSTABLE.
            Pipeline execution will by default proceed even when the build is unstable. 

            As per: https://www.jenkins.io/doc/pipeline/tour/tests-and-artifacts/
            To skip deployment after test failures in Declarative syntax, use the skipStagesAfterUnstable option. 
            In Scripted syntax, you may check currentBuild.currentResult == 'SUCCESS'. */
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed.'
            echo 'For example, if the Pipeline was previously failing but now succeeds.'
        }
    }
}
