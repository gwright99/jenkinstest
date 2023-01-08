/* Jenkinsfile (Declarative Pipeline) */
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
        /* Get human input before continuing on to something more dangerous like deploying to a higher environment */
        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        /* 
        https://www.jenkins.io/doc/pipeline/tour/deployment/
        One common pattern is to extend the number of stages to capture additional deployment environments, like "staging" or "production".
        This kind of pipeline that automatically deploys code all the way through to production can be considered an implementation of "continuous deployment." 
        While this is a noble ideal, for many there are good reasons why continuous deployment might not be practical, but those can still enjoy the 
        benefits of continuous delivery.
        */
        stage('Deploy - Staging') {
            steps {
                sh './deploy staging'
                sh './run-smoke-tests'
            }
        }
        stage('Deploy - Production') {
            steps {
                sh './deploy production'
            }
        }
    }
}
