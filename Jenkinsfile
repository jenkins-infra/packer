#!/usr/bin/env groovy


node('docker') {
    checkout scm

    docker.image('golang:1.6-wheezy').inside {
        /* Ensure that we have a directory in the right place so Go can find
         * us
         */
        sh 'mkdir -p $GOPATH/src/github.com/mitchellh'
        sh 'cp -R $PWD $GOPATH/src/github.com/mitchellh/packer'

        withEnv([
            'GOMAXPROCS=2',
        ]) {
            stage 'Build'
            sh 'cd $GOPATH/src/github.com/mitchellh/packer && make deps && ./scripts/build.sh'

            stage 'Archive'
            sh 'cp -R $GOPATH/src/github.com/mitchellh/packer/pkg .'
            archiveArtifacts artifacts: 'pkg/**/*.tar.gz', fingerprint: true
        }
    }
}
