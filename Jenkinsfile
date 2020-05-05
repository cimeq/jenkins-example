pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'm3') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'm3') {
                    sh 'mvn test'
                }
            }
        }

        stage('Approval') {
            timeout(time:3, unit:'DAYS') {
                input 'Do I have your approval for deployment?'
            }
        }

        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'm3') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}
