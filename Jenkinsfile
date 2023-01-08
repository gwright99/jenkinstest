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
        }
        success {
            echo 'This will only run if the stages executed successfully.'
        }
        failure {
            echo 'This will only run when at least one of the stages failed.'
        }
        unstable {
            echo 'This will only run if the run was marked as unstable.'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed.'
            echo 'For example, if the Pipeline was previously failing but now succeeds.'
        }
    }
}
