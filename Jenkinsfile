pipeline {
    agent { label 'docker-server-dimas	' }
    
    tools {nodejs "NodeJS-18.16.0"}
environment {
    NAMEAPPS = 'Simple-apps-pipeline-apps'
    SONARHOST = 'http://172.23.11.114:9000'
    TOKENSONAR = 'sqp_21e5e0bb9e43f9c5fc9376b5c60fc7e9e5edc3f0'
    VERSION = 'v1'

}
    stages {
        stage('Build') {
            steps {
                sh '''
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''
                npm test'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=${SONARHOST} \
                -Dsonar.login=${TOKENSONAR}'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
      stage('tagging and push image to registry image') {
        steps {
          sh ''' 
          docker tag ${NAMEAPPS} sayadimas/${NAMEAPPS}:${VERSION}
          docker push sayadimas/${NAMEAPPS}:${VERSION}
          docker images prune -a -f
          '''
        }
      }
    }
}
