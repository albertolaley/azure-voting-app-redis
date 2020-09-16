pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build') {
         steps {
            sh 'docker images -a'
            sh '''
                cd azure-vote/
                docker images -a
                docker build -t jenkins-pipeline .
                docker images -a
                cd ..            
            '''
         }
      }
        stage('Start test app') {
         steps {
             sh '''
             chmod 777 docker-compose.yml
             echo "docker-compose up -d"
             '''
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            sh '''
               echo "Test runned"
            '''
         }
      }
      stage('Stop test app') {
         steps {
            sh '''
               echo "docker-compose down"
            '''
         }
      }
      stage('Push Container') {         
         steps {
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote") {
               script {
                  docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                  def image = docker.build('albertolaley/jenkins-course:latest')
                  image.push()
               }
            }
           }
         }
      }
    }
}