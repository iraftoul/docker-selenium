pipeline {
    environment {
        def proj = "deploy-test"
        def dir = "StandaloneChrome"
        def app = "selenium-standalone"
        //def bc = "selenium-build"
        //def dc = "selenium-deploy"
    }
    agent any
    stages {

        stage('Checkout git sources') {
            steps {
                dir("${dir}") {
                    git 'https://github.com/iraftoul/docker-selenium.git'
                }
            }
        }

        stage('Build docker image') {
            steps {
                dir("${dir}") {
                    script {
                        openshift.withCluster() {
                            openshift.withProject("${proj}") {

                                //sh "cat Dockerfile | oc new-build -D - --name ${bc}"
                                //sh "oc apply -f ${bc}"

                            }
                        }
                    }
                }
            }
        }

        stage('Deploying image') {
            steps {
                dir("${dir}") {
                    script {
                        openshift.withCluster() {
                            openshift.withProject("${proj}") {

                                //sh "oc apply -f ${dc}"
                                sh "oc project ${proj}"
                                sh "oc new-app selenium/standalone-chrome:4.0.0-rc-2-prerelease-20210908  --name ${app}"
                                sh "oc expose svc/${app}"

                            }
                        }
                    }
                }
            }
        }

    }
}
