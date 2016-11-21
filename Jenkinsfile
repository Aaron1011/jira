#!groovy

timestamps {

    ansiColor('xterm') {

        node {

            // -------------------------------------------------------------------------
            stage('build') {
                checkout scm

                buildVersion = sh(
                    script: 'python setup.py --version',
                    returnStdout: true
                ).split("\r?\n")[0]

                // Set build metadata
                currentBuild.displayName =  "#" + env.BUILD_ID + " " + env.BRANCH_NAME
                currentBuild.description = buildVersion + ' on ' + env.BRANCH_NAME

                //sh 'pip install -q tox'
                /*
                parallel(
                   py27: { sh 'tox -e py27'},
                   py34: { sh 'tox -e py34'},
                   py35: { sh 'tox -e py35'},
                   docs: { sh 'tox -e docs'}
                )
                */

                sh 'tox -e py27'
                sh 'tox -e py34'
                sh 'tox -e py35'
                sh 'tox -e docs'

                archive '.tox/*/log/*.log'

                // We only push the image to the Docker Hub if we're on master
                if (env.BRANCH_NAME != 'master') {
                    return
                }
            }
            // -------------------------------------------------------------------------
            stage('push') {
              echo 'yep'
            }
        }
    }
}
