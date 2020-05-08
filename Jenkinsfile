pipeline {
    agent any
    stages {
        stage('origin-branch-stuff'){
            agent any
            when{
                branch 'origin/*'
                beforeAgent true
            }
            steps {
                echo 'run this stage - ony if the branch = origin somting branch'
            }
        }
   
        stage('testmaster-branch-stuff'){
            agent any
            when {
                expression {env.GIT_BRANCH == 'origin/master'}
            }
            steps {
                echo 'run this test stage - ony if the branch = master branch'
            }
        }

        stage('master-branch-stuff'){
            agent any
            when{
                branch 'master'
                beforeAgent true
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
