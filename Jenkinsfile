#!/usr/bin/env groovy


node('docker') {
    checkout scm

    /* mapping /etc/passwd into the container so `git` (invoked by `go get` can
     * properly resolve a username to perform a clone -_-
     */
    docker.image('golang:1.6-wheezy').inside('-v /etc/passwd:/etc/passwd') {
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
            archiveArtifacts artifacts: 'pkg/**/packer', fingerprint: true
        }
    }
}
