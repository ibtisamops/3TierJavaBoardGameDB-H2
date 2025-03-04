pipeline {
    agent any
    
    tools {
        maven "maven3"
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner-tool' // tool name configured in Jenkins
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ibtisam-iq/3TierJavaBoardGameDB-H2.git'
            }
        }
        
        stage('Compile') {
            steps {
               //dir('03.Projects/00.LocalOps/0.1.01-jar_Boardgame') {
                    sh "mvn compile"
                
            }
        }
        
        stage('Test') {
            steps {
               //dir('03.Projects/00.LocalOps/0.1.01-jar_Boardgame') {
                    sh "mvn test"
                    
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                //dir('03.Projects/00.LocalOps/0.1.01-jar_Boardgame') {
                    withSonarQubeEnv('sonar-server') { // server name configured in Jenkins
                        sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=IbtisamX \
                        -Dsonar.projectKey=ibtisamx \
                        -Dsonar.java.binaries=target \
                        -Dsonar.branch.name=ibtisam
                        '''
                    }
            
            }
        }
        
        stage('Quality Gate Check') {
            steps {
                //dir('03.Projects/00.LocalOps/0.1.01-jar_Boardgame') {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                
            }
        }
        
        stage('Package') {
            steps {
                //dir('03.Projects/00.LocalOps/0.1.01-jar_Boardgame') {
                    sh "mvn package"
                
            }
        }
        
        stage('Love') {
            steps {
                echo 'Love you, Sweetheart'
            }
        }
    }

    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
    }
}
