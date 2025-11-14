pipeline {
    agent any

    stages {
        stage('Checkout portfolio repository') {
            steps {
                git branch: 'main', url: 'https://github.com/timo-nsk/timo-nsk-github-io'
            }
        }

        stage('Build project') {
            steps {
                sh 'ng b --output-path docs --base-href /timo-nsk-github-io/'
            }
        }

        stage('Move files') {
            steps {
              dir('docs') {
                sh 'mv browser/* .'
                sh 'rm -r browser'
              }
            }
        }

        stage('Push new build to repository') {
            steps {
                sh '''
                  git add .
                  git commit -m "Automated build update" || true
                '''

                withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                      git push https://$GITHUB_TOKEN@github.com/timo-nsk/timo-nsk-github-io.git main
                    '''
                }
            }
        }
    }
}
