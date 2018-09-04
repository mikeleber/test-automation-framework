pipeline {
    agent {
        label 'common'
    }

    parameters {
        booleanParam(defaultValue: false, description: 'Flag whether a release should be created.', name: 'IS_RELEASE')
    }

    tools {
        jdk 'JDK_1_8'
    }

    options {
        skipStagesAfterUnstable()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '28'))
        timeout(time: 1, unit: 'HOURS')
    }

    triggers {
        // at least once a day
        cron('H 12 * * *')
        // every sixty minutes
        pollSCM('H/60 * * * *')
    }

    stages {
        stage("SCM Checkout") {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage("Maven") {
            steps {
                script {
                    if (params.IS_RELEASE) {
                        release()
                    } else {
                        mavenbuild()
                    }
                }
            }
        }
    }
}
