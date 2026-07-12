pipeline {
    agent { label 'dimas-job' }
    
    tools {nodejs "NodeJS-18.16.0"}

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
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.11.114:9000 \
                -Dsonar.login=sqp_21e5e0bb9e43f9c5fc9376b5c60fc7e9e5edc3f0'''
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
          docker tag simple-apps-pipeline sayadimas/simple-apps-pipeline
          docker push sayadimas/simple-apps-pipeline
          docker images prune -a -f
          '''
        }
      }
    }
}
