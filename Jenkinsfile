
pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Recuperation du projet depuis GitHub'
                checkout scm
            }
        }

        stage('Tests unitaires Backend') {
            steps {
                dir('backend') {
                    echo 'Execution des tests unitaires'
                    sh 'mvn -B clean test'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    echo 'Construction du backend Spring Boot'
                    sh 'mvn -B package -DskipTests'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build termine avec succes !'
        }

        failure {
            emailext(
                subject: "ECHEC Jenkins : ${env.JOB_NAME}",
                body: """
                Bonjour,

                La construction du backend a echoue.

                Projet : ${env.JOB_NAME}
                Build : ${env.BUILD_NUMBER}
                Details : ${env.BUILD_URL}

                Veuillez consulter les logs Jenkins.

                """,
                to: 'TON_EMAIL@example.com'
            )
        }
    }
}
