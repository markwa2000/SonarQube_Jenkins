pipeline{
    agent any
    
    environment{    
        nodeVersion = '20.8.1'  // Specify your desired Node.js version
    }

    stages{
        stage('Checkout'){
            steps{
                // Checkout the source code from your version control system (e.g., Git)
                checkout([$class: 'GitSCM', branches: [[name: 'MAIN']], extensions: [], userRemoteConfigs: [[credentialsId: 'PAT_Jenkins', url: 'GITHUB_URL']]])
            }
        }

        stage('Set Node.js Version'){
            steps {
                script {
                    def nvmPath = "/bitnami/jenkins/home/.nvm/versions/node/${nodeVersion}/bin"
                    env.PATH = "${nvmPath}:${env.PATH}"
                }
            }
        }
        
        stage('Install Dependencies'){
            steps {
                // Install project dependencies
                nodejs(nodeJSInstallationName: 'nodejs'){
                    sh 'npm install'
                    withSonarQubeEnv('sonarqube'){
                      sh "npm install sonar-scanner"
                      sh "npm run sonar"
                    }
                }
            }
        }

        stage('Clean node_modules'){
            steps {
                sh 'sudo rm -rf node_modules'
            }
        }
    }
}
