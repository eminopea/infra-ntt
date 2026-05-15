pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: 'latest', description: 'Versión a desplegar')
    }

    stages {

        stage('Checkout Infra') {
            steps {
                dir('infra') {
                    git url: 'https://github.com/eminopea/infra-ntt.git', branch: "${env.BRANCH_NAME}"
                }
            }
        }

        stage('Deploy DEV') {
            when { branch 'develop' }
            steps {
                sh """
                cd infra
                VERSION=${params.VERSION} docker compose -f docker-compose.dev.yml --env-file .env.dev up -d
                """
            }
        }

        stage('Deploy QA') {
            when { branch 'qa' }
            steps {
                sh """
                cd infra
                VERSION=${params.VERSION} docker compose -f docker-compose.qa.yml --env-file .env.qa up -d
                """
            }
        }

        stage('Deploy PROD') {
            when { branch 'main' }
            steps {
                input message: "¿Deploy a producción?"

                sh """
                cd infra
                VERSION=${params.VERSION} docker compose -f docker-compose.prod.yml --env-file .env.prod up -d
                """
            }
        }
    }
}