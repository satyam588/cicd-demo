pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS' // Ensure NodeJS is set up in Jenkins Global Tool Configuration
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/satyam588/cicd-demo.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        // stage('Test') {
        //     steps {
        //         sh 'npm test'
        //     }
        // }
        
        stage('Deploy') {
            steps {
                sshPublisher(
                    publishers: [sshPublisherDesc(
                        configName: 'Crazybooks.in',
                        transfers: [sshTransfer(
                            cleanRemote: false,
                            excludes: '',
                            execCommand: 'rm -rf /var/www/html/* && mv /var/www/html/build/* /var/www/html/',
                            execTimeout: 120000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: '/home/admin/web/cryptopay.crazybooks.in/public_html',
                            remoteDirectorySDF: false,
                            removePrefix: 'build/',
                            sourceFiles: 'build/**/*'
                        )],
                        usePromotionTimestamp: false,
                        useWorkspaceInPromotion: false,
                        verbose: false
                    )]
                )
            }
        }
    }
}
