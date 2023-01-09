/* Jenkinsfile (Declarative Pipeline) */
/* Conditional checks via `when` block - I only want to run stage test on certain branches */
/* Active branch name in the build is always available via Jenkins-provided environment variable BRANCH_NAME.
        See all Jenkins-supplied envvars at http://JENKINS_URL/env-vars.html/ */

CODE_CHANGES = getGitChanges()  /* this would be some groovy script that checks if something changed. */
/* variable defined outside pipeline block, so it's globally available. */
def gv

pipeline {
    agent { label 'linux' }

    tools {
        maven    /* has to be already install in Jenkins */
    }

    /* How to pass in external config values to the build */
    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod.')
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.p'], description: '')
        booleanParam(name: 'executeTests', default=true, description: '')
    }

    /* define my own environment variables */
    environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('xxx')  /* Made availabl by 'Credentials Binding Plug=in'. Available to ALL stages. */
    }

    stages {
        stage('init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage('use_groovy_function') {
            steps {
                script {
                    gv.doSomething()
                }
            }
        }

        stage('build') {
            when {
                expression {
                    env.BRANCH_NAME == 'dev' && CODE_CHANGES == true
                }
            }
            steps {
                echo 'building the application ...'
                echo "building version ${NEW_VERSION}"  /* Groovy syntax re interpolation */
            }
        }

        stage('test') {
            
            when {
                expression {
                    /* Dont run stage test if branch isn't `dev` or `master` */
                    env.BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
                    params.executeTests == true
                }
            }
            steps {
                echo 'testing the application ...'
                echo "testing with ${SERVER_CREDENTIALS}"
                sh "${SERVER_CREDENTIALS}"
            }
        }

        stage('deploy') {
            steps {
                echo 'deploying the application ...'
                /* If you only need credentials in one block, use `withCredentials` wrapper.
                This examples assumes there's a credential of type 'usernamePassword' already efined in Jenkins. 
                This looks weird cuz it's Groovy. */
                withCredentials([
                    usernamePassword(
                        credentials: 'server-credentials', 
                        usernameVariable: USER,
                        passwordVariable: PWD 
                    )
                ]) {
                    sh "some script ${USER} ${PWD}"
                }
                echo "deploying with ${SERVER_CREDENTIALS}"
                echo "deploying version ${VERSION}"
                sh "${SERVER_CREDENTIALS}"
            }
        }
    }
}
