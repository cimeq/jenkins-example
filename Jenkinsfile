pipeline {
    agent any
    parameters 
    {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        
        choice(name: 'PerformMavenRelease', choices: 'False\nTrue', description: 'Whether or not to perform a maven release')
        credentials(name: 'CredsToUse', description: 'A user to build with', defaultValue: '', credentialType: "Username with password", required: true )           
    }
//     environment {
//        BUILD_USR_CREDS = credentials("${params.CredsToUse}")
//    }  
    stages {
        stage('Example') 
        {
            steps 
            {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

//                echo "Password: ${params.PASSWORD}"
            }
        }
//    }
//    stages {
        stage('debugStep'){
            steps {
                sh 'printenv'
            }
        }
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
                beforeAgent true
            }
            steps {
                echo 'run this test stage - ony if the branch = master branch'
            }
        }

        stage('master-branch-stuff'){
            agent { node { label 'master' } }
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
