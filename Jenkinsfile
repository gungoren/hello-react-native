pipeline {
  agent none

  options {
    disableConcurrentBuilds()
    timeout(time: 60, unit: 'MINUTES')
    timestamps()
  }

  stages {
    stage("Build"){
        parallel {
            stage("android"){
                agent any
                stages {
                    stage("android - Checkout") {
                        steps {
                            checkout scm
                        }
                    }
                    stage("android - Install Dependencies") {
                        steps {
                            sh """
                            rm -rf node_modules/
                            npm i
                            """
                        }
                    }
                    stage("android - Build") {
                        steps {
                            dir("android"){
                                sh "./gradlew assembleDebug"
                            }
                        }
                    }
                    stage("android - Deploy") {
                        when {
                            expression { BRANCH_NAME ==~ "^(main|master|develop)\$" }
                        }
                        steps {
                            dir("android"){
                                sh "ls "
                            }
                            echo "Hey, look, I'm echoing with a timestamp! ${STAGE_NAME}"
                        }
                    }
                }
            }
            stage("ios"){
                agent any // change this to mac slave
                stages {
                    stage("android - Checkout") {
                        steps {
                            checkout scm
                        }
                    }
                    stage("android - Build") {
                        steps {
                            echo "Hey, look, I'm echoing with a timestamp! ${STAGE_NAME}"
                        }
                    }
                    stage("android - Deploy") {
                        when {
                            expression { BRANCH_NAME ==~ "^(main|master|develop)\$" }
                        }
                        steps {
                            echo "Hey, look, I'm echoing with a timestamp! ${STAGE_NAME}"
                        }
                    }
                }
            }
        }
    }
  }
}
