#!/usr/bin/env groovy


node('docker') {
    docker.image('go:1.6-alpine') {
        withEnv([
            'GOMAXPROCS=2',
        ]) {
            stage 'Install Dependencies'
            sh 'make deps'

            stage 'Build'
            sh 'make ci'
        }
    }
}
