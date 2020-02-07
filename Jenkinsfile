pipeline {
    agent any
    stages {
        stage('Build and install') {
            steps {
                echo "Step 1: Installing dependencies"
                sh 'npm ci'
            }
        }

        stage('Running test') {
            steps {
                echo "Step 1: Test"
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-registry', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh ' echo "$PASSWORD" | docker login --username=$USERNAME --password-stdin'
                    echo "Step 1: Build Docker image"
                    sh 'docker build -t glgelopfalcon/angular-realworld:${BUILD_NUMBER} .'
                    sh 'docker push glgelopfalcon/angular-realworld:${BUILD_NUMBER}'
            }
         }
        }

        stage('Deploy to DEV') {
            steps {
                sh 'sed -i -e "s/\\/angular-realworld:.*/\\/angular-realworld:${BUILD_NUMBER}/" angular-realworld.yml'
                sh 'sudo kubectl apply -f  angular-realworld.yml'
            }
        }
        
      stage('Running Integration test') {
            steps {
                echo "Step 1: INT test"
            }
      }
      
      stage('Prune Docker Images') {
            steps {
                echo 'Step 1: Prune images'
                sh 'docker image prune -a -f'
            }
        }
    }
}
