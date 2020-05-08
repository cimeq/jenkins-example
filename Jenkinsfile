pipeline {
    agent any
    
    stages {
        stage('master-branch-stuff'){
            agent any
            when{
                branch 'master'
            }
            steps {
                echo 'run this stage - ony if the branch = master branch'
            }
        }

        stage ('Compile Stage') {

            steps {
                withEnv(["PATH+MAVEN=${tool 'm3'}/bin"]) {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withEnv(["PATH+MAVEN=${tool 'm3'}/bin"]) {
                    sh 'mvn test'
                }
                jacoco()
            }
        }

        stage ('Approval') {
            steps {
            timeout(time:3, unit:'DAYS') {
                input 'Do I have your approval for deployment?'
            }
            }
        }

       // stage ('Deployment Stage') {
       //     steps {
       //         withEnv(["PATH+MAVEN=${tool 'm3'}/bin"]) {
       //             sh 'mvn deploy'
       //         }
       //     }
       // }
    }
}
