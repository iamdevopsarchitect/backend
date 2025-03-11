pipeline {
    agent {
        label 'AGENT-2'
    }
    options{
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global, we can use across pipeline.
    }
    stages {
        stage('Read the version') {
            steps {
                script{
                    def packageJson = readJSON file: 'package.json' //defining variable with def
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
                    
            }
        }
        stage('Install Dependencies') {
            steps {
                    sh 'npm install'
            }
        }
        stage('Docker build') {
            
            steps {
                    sh """
                    docker build -t iamdevopsarchitect:${appVersion}
                    docker images
                    """
                    
            }
        }
        stage('Print Params'){
            steps{
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"
            }
        }

    }

    post {
        always { 
            echo 'This section runs always'
            deleteDir()
        }
        success { 
            echo 'This section runs when pipeline success!'
        }
        failure { 
            echo 'This section run when pipeline failure!'
        }
    }
}