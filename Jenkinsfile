pipeline {
    agent any

    tools {
        nodejs 'nodejs23' // Must match the name in Global Tool Configuration
    }

    // environment {
    //     SCANNER_HOME = tool 'sonar-scanner' // Also must be pre-configured
    // }

    stages {

        stage('Cloning Git Repository') {
            steps {
                git branch: 'dev', url: 'https://github.com/infraghost/3-Tier.git'
            }
        }

        stage('Frontend Compilation') {
            steps {
                dir('client') {
                    // Use --check (not --checkout) to validate JS syntax
                    sh 'find . -name "*.js" -exec node --check {} +'
                }
            }
        }

        stage('Backend Compilation') {
            steps {
                dir('api') {
                    sh 'find . -name "*.js" -exec node --check {} +'
                }
            }
        }

        stage('Scanning Git Leaks') {
            steps {
                sh 'gitleaks detect --source ./client --exit-code 1'
                sh 'gitleaks detect --source ./api --exit-code 1'
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('sonar') {
        //             sh '''
        //                 $SCANNER_HOME/bin/sonar-scanner \
        //                 -Dsonar.projectKey=NodeJS-Project \
        //                 -Dsonar.projectName=NodeJS-Project \
        //                 -Dsonar.sources=.
        //             '''
        //         }
        //     }
        // }

        // stage('Quality Gate Check') {
        //     steps {
        //         timeout(time: 1, unit: 'HOURS') {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
        //         }
        //     }
        // }

        // stage('Trivy FS Scan') {
        //     steps {
        //         sh 'trivy fs --format table -o fs-report.html .'
        //     }
        // }
    }
}
