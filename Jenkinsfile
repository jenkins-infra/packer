#!/usr/bin/env groovy


node('docker') {
    checkout scm

    docker.image('golang:1.6-wheezy').inside {
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
