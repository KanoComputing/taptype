#!groovy

pipeline {
    agent any
    stages {
        stage('check environment') {
            steps {
                prepare_env()
            }
        }

        stage('checkout') {
            steps {
                checkout scm
            }
        }

        stage('install dependencies') {
            steps {
                install_dep()
            }
        }

        stage('build') {
            steps {
                run_build()
            }
        }

        stage('deploy') {
            steps {
                deploy(env.BRANCH_NAME)
            }
        }
    }
}

def prepare_env () {
    if (env.BRANCH_NAME == "production") {
        env.DEV_ENV = "production"
    } else {
        env.DEV_ENV = "staging"
    }
    env.NODE_ENV = "${env.DEV_ENV}"
}

def run_build () {
    sh "npm run build"
}

def install_dep () {
    sh "npm i"
    sh "npm run bower-setup"
}

def deploy(branch) {
    def channel = "production"

    if (branch == "master") {
        channel = "staging"
    }

    sh "npm run deploy-${channel}"
}

