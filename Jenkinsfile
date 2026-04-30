pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    parameters {
        string(name: 'TOMCAT_URL', defaultValue: 'http://localhost:9090', description: 'Base URL of the Tomcat server')
        string(name: 'TOMCAT_CONTEXT', defaultValue: 'calculator', description: 'Tomcat context path used for deployment')
        string(name: 'TOMCAT_CREDENTIALS_ID', defaultValue: 'tomcat-manager', description: 'Jenkins credentials ID for Tomcat Manager')
    }

    environment {
        APP_NAME = 'calculator'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn -B clean test'
                    } else {
                        bat 'mvn -B clean test'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn -B -DskipTests package'
                    } else {
                        bat 'mvn -B -DskipTests package'
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(credentialsId: params.TOMCAT_CREDENTIALS_ID, usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                    script {
                        def deployUrl = "${params.TOMCAT_URL}/manager/text/deploy?path=/${params.TOMCAT_CONTEXT}&update=true"
                        if (isUnix()) {
                            sh "curl --fail --show-error --silent -u \"$TOMCAT_USER:$TOMCAT_PASSWORD\" --upload-file \"target/${env.APP_NAME}.war\" \"${deployUrl}\""
                        } else {
                            bat "curl --fail --show-error --silent -u %TOMCAT_USER%:%TOMCAT_PASSWORD% --upload-file \"target\\${env.APP_NAME}.war\" \"${deployUrl}\""
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
    }
}