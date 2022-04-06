pipeline {
    agent any
    environment {
		  DOCKERHUB_CREDENTIALS=credentials('samer-skillx-docker')
    }
    stages {
        stage('Clean') {
            steps {
                sh 'docker rm -f $(docker ps -a -q) || true'
            }
        }
      stage('Build') {
            steps {
                sh 'docker build --tag helloworld:$BUILD_NUMBER .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
      stage('Tag') {
            steps {
                sh 'docker tag helloworld:$BUILD_NUMBER samernasser/hk-helloworld:$BUILD_NUMBER'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push samernasser/kh-helloworld:$BUILD_NUMBER'
            }
        }
    }
  post {
	  always {
		  sh 'docker logout'
	  }
  }
}
