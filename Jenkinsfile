#!/usr/bin/env groovy


node('docker') {
    checkout scm

    docker.image('golang:1.6-wheezy').inside {
        /* Ensure that we have a directory in the right place so Go can find
         * us
         */
        sh 'export PKGDIR=$GOPATH/src/github.com/mitchellh; mkdir -p $PKGDIR && ln -s $PWD $PKGDIR/packer'

        withEnv([
            'GOMAXPROCS=2',
        ]) {
            stage 'Build'
            sh 'cd $GOPATH/src/github.com/mitchellh/packer && make ci'
        }
    }
}
