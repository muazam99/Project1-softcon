pipeline {
     agent any
     environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub_id')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'docker build -t burhan99/project1:latest .'
            }
            post {
                always {
                    jiraSendBuildInfo branch: 'master', site: 'groupgradient.atlassian.net'
                }
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push burhan99/project1:latest'
            }
        }
        stage('Deploy - Production') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to Production from master...'
            }
            post {
                always {
                    jiraSendDeploymentInfo environmentId: '', environmentName: '', environmentType: 'development', issueKeys: [''], serviceIds: [''], site: 'groupgradient.atlassian.net', state: 'unknown'
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
