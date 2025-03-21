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
                agent {
                    docker {
                        image "reactnativecommunity/react-native-android"
                    }
                }
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
                            sh "npm run bundle:android-release"
                            dir("android"){
                                sh "./gradlew assembleDebug --build-cache --no-daemon"
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
                    /*stage('android - Publish') {
                        when {
                            expression { BRANCH_NAME ==~ "^(main|master|develop)\$" }
                        }
                        environment {
                            APPCENTER_API_TOKEN = credentials('at-this-moment-you-should-be-with-us')
                        }
                        steps {
                            appCenter apiToken: APPCENTER_API_TOKEN,
                                    ownerName: 'janes-addiction',
                                    appName: 'ritual-de-lo-habitual',
                                    pathToApp: 'three/days/xiola.apk',
                                    distributionGroups: 'casey, niccoli'
                        }
                    }*/
                }
            }
            stage("ios"){
                agent any // change this to mac slave
                stages {
                    stage("ios - Checkout") {
                        steps {
                            checkout scm
                        }
                    }
                    stage("ios - Build") {
                        steps {
                            echo "Hey, look, I'm echoing with a timestamp! ${STAGE_NAME}"
                        }
                    }
                    stage("ios - Deploy") {
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
